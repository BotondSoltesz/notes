# Scalable Web Applications
## Traditional web hosting
- web tier
- app server tier
- data tier

            (o) 
             |
             V
             O load balancer
             |
         +---+---+
         |       |
         V       V
        .-.     .-.
        | |     | | web servers
        '-'     '-'
         |       |
         +---+---+
             |
             V
             O load ballancer
             |
         +---+---+     
         |   |   |
         V   V   V
        .-. .-. .-.
        | | | | | | app servers
        '-' '-' '-'
         |   |   |
         +---+---+
             |
             V
             -       -
         db |_|<--->|_| slave db

### Challenges
- requires acurate forecasts
- sudden traffic peaks
- low utilization rate of expensive hardware
- high maintenance cost for idle hardware
                      