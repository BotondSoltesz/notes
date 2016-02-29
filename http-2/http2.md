# Http/2 Connection
## Tcp
- reliable
- in order for tcp to be reliable: flow control
    - slow start
    - starts with small window size (transfer rate)
    - raises window gradually
    - this results in high cost of building up a connection

## Http/2
- binary (instead of textual)
- same semantics as http 1.x
- more about solving shortcomings of tcp than http/1.x
    - main concern is increase performance
- hpack - header compression

## Http 1.x connection
### Request

        GET /index.html HTTP 1.0
        User-Agent: mozila
        Accept: text/html

### Response

        HTTP/1.0 200 OK
        Date: Fri, 31 Dec 1999 23:59:59 GMT
        Content-Type: text/html
        Content-Length: 1354
        
        <html>
            <body>
                <h1>Hello, World!</h1>
            </body>
        </html>

## Http/2 connection
- exchange is binary
- binary messages contain the same as http/1.x in different representation
    - verb
    - resource
    - headers
- needs to work on existing infrastructure - Http/2 over Http 1.x
    - plaintext or
    - TSL

### Connection
| Request | Response | notes |
| --- | --- | --- | 
| `Upgrade: h2c` <br /> `HTTP2-Settings: ...` | ... | ... |
| ... | `101 Switching Protocols` <br /> `Connection: Upgrade` <br /> `Upgrade: h2c` | If server is http/2 capable, it responds with 101 |

**or** negotiating connection with TLS + ALPN

- ALPN: Application Layer Protocol Negotiation
- extension protocol
- protocol negotiation during TLS handshake
- most servers choose to implement http/2 with tls only (no plaintext)

### Frames
- basic protocol unit
- semantics are preserved from HTTP 1.x
    - Headers are sent in header frames
    - body is sent in data frames
- 10 types of frames
    - DATA
        - 8 bits Pad Length + Data + Padding
    - HEADERS
    - PRIORITY
    - RST_STREAM
    - SETTINGS
    - PUSH_PROMISE
    - PING
    - GOAWAY
    - WINDOW_UPDATE
    - CONTINUATION


        +-----------------------------------+
        | Length (24 bits)                  |
        +---------------+-----------------+-+
        | Type (8 buts) | Flags (8 bits)  |
        +---+-----------+-----------------+-----+
        | R | ID (31 bits)                      |
        +---+-----------------------------------+
        | Payload (2^14 - 2^24-1 octets)        |
        +---------------------------------------+

#### Data Frame Payload

        +--------------------+
        | Pad length (8 bits |
        +--------------------+------------+
        | Data                            |
        +---------------------------------+
        | Padding                         |
        +---------------------------------+

#### Header Frame Payload
- Headers may be sent in multiple frames
    - Flags: EndHeaders, EndStream indicate if frame is last
    - or followed by CONTINUATION frames


        +--------------------+
        | Pad length (8 bits |
        +---+----------------+------------+
        | E | Stream Dependency (31 bits) |
        +---+-------------+---------------+
        | Weight (8 bits) |
        +-----------------+---------------------+
        | Header Block Fragment                 |
        +---------------------------------------+
        | Padding                               |
        +---------------------------------------+
        
- header fields are case-insensitive
    - but they are required to be converted to lower case before encoding
- regular header fields: key value pairs
- pseudo header fields
    - pseudo header fields must appear **before** all other header fields
    - prefixed with colon
    - `:method`
    - `:scheme`
    - `:authority`
    - `:path`
    - `:status`

#### HPAC: Header Compression
- Huffmann encoding with
- a Static Table
    - usual headers
- a Dynamic table
    - initially empty
    - maximum size set in settings frame's `SETTINGS_HEADER_TABLE_SIZE`
    - connection specific headers
- Static and Dynamic tables constute the *Index Address Space*
- instead of sending header names & values, HPACK uses indexes to look up corresponding names & values
    - maintains state for the headers

#### Settings Frame Payload
- `SETTINGS_HEADER_TABLE_SIZE`
- `SETTINGS_ENABLE_PUSH`
- `SETTINGS_MAX_CONCURRENT_STREAMS`
- `SETTINGS_INITIAL_WINDOW_SIZE`
- `SETTINGS_MAX_FRAME_SIZE`
- `SETTINGS_MAX_HEADER_LIST_SIZE`


        +----------------------+
        | Identifier (18 bits) |
        +----------------------+----------+
        | Value (32 bits)                 |
        +---------------------------------+

# Streams
## Comparison

| Http 1.x | Http/2 |
| --- | --- |
| Text Protocol | Binary Protocol |
| Verb and Resource Semantics | Verb and Resource Semantics |
| Headres and Body | Headers and Body |
| Hedares Repeated | Headers Optimized |
| SSL Optional | SSL Optional but mostly used |
| Single Connection - Single Request | Single Connection - Multiple Requests Multiplexed |

- Http 1.x usually opens up multiple connections to access multiple resources
- Http/2 multiplexes requests over a single connection

## Multiplexing
- Multiple signals are combined into a single signal for transport
- multiplexed on sender's end
- demultiplexed on receiver's end

## Http/2 Streams
- Multiplexing in Http/2
    - multiple requests and responses share a single connection
    - achieved by having multiple streams over a single connection
    - bidirectional!
- streams are identified by an integer
- a stream is a set of frames exchanged between client and server sharing the same id
    - client to server initiated streams have **odd** id numbers
    - server to client initiated streams have **even** id numbers
    - stream 0 is used for connection control messages
    - stream id is declared in the frame's ID field
- Streams bring parallel requests / responses
    - maximum number of concurrents streams set in SETTINGS frame's `SETTINGS_MAX_CONCURRENT_STREAMS`
    - specific to each endpoint and applicable to the peer only that receives the settings
- Streams have states
    - idle
    - open
    - half closed (remote)
    - half closed (local)
    - reserved (remote)
    - reserved (local)
    - closed

### Stream Priorities
- ability to prioratize stream processing
- stream is assigned a `weight` 1..256 (lower number has higher prio
- `dependency: {stream id}`; no dependency: id set to 0 (root)
- define priority by
    - Priority flag in HEADERS frame, or
    - PRIORITY frame during exchange when changing priority of stream
        - nothing guarantees the order in which PRIORITY frame is processed
- priorities are only recommendations
- `E` Exclusive flag in HEADER and PRIORITY frames
    - indicates whether this frame is the only one dependent on its parent
    - useful for inserting higher priority frames during exchange
    - example
        - Stream 3 & Stream 4 have dependency on Stream 1
        - Stream 2 has dependency on Stream 4
        - Stream 5 has dependency on Stream 1 and has E Exclusive flag set


        .-----------.                                   .----------.
        |  Stream 1 |                                   | Stream 1 |
        '-----------'                                   '----------'
              A                                               A
              |                                               |
              +------------+         Setting E flag     .----------.
              |            |         --------------->   | Stream 5 |
        .----------.  .----------.   on stream 5        '----------'
        | Stream 3 |  | Stream 4 |                            A
        '----------'  '----------'                            |
                           A                           +------+------+
                           |                           |             |
                      .----------.               .----------.   .----------.
                      | Stream 2 |               | Stream 3 |   | Stream 4 |
                      '----------'               '----------'   '----------'
                                                                     A
                                                                     |
                                                                .----------.
                                                                | Stream 2 |
                                                                '----------'

### Flow Control
- Similar to the one implemented in TCP layer
- initial Window size is set to 64 KB
    - receiver can increase / decrease window size based on how much they can processe
    - sender must respect window size
- available at 
    - connection level
        - in the SETTINGS frame
    - or at stream level
        - using the WINDOW_UPDATE frame
- flow control is hop-to-hop

# Server Push