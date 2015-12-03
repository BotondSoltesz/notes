# Committee Prep

## What principles do you follow?
- SOLID
	- **S**ingle responsibility principle: a class should hava a single reason to change
	- **O**pen/closed principle: open for extension, closed for modification; allow the changing of its behaviour without modifying the source code
	- **L**iskov substitution: objects should be replaceable with instances of their subtype without altering the correctness of the program - design by contract
	- **I**nterface segregation principle: many client specific interfaces are better than one general purpose monolithic one
	- **D**ependency inversion principle: depend upon abstractions, not concrete implementations
- YAGNI: You Ain't Gonna Need It
- KISS: Keep It Simple, Stupid
- DRY: Don't Repeat Yourself

## How do you keep the code clean?
- What is clean code?
	- easy to read
	- easy to modify
	- does one thing and does it well
	- thoroughly tested
	- author cares - craftsmanship
	
- How to measure Quality?
	- Wtf / Minute
	- abstractness vs. instability
		- see http://www.objectmentor.com/resources/articles/oodmetrc.pdf
		- afferent coupling (Ca): number of external types that depend on this project
		- efferent coupling (Ce): number of external types that this project depends on
		- instability (I = Ce / (Ca + Ce)): percentage of efferent coupling
			- shows the likelihood of a change cascading into other classes beyond control 
			- or in other words: stability shows the reusability of the component
	- cyclomatic complexity
		number of linearly independent execution paths in a program
	- test coverage (line, branch)
	- defect density
	- fan in / fan out
		
- What are code smells?
	- telltale signs that there are structural problems in the code
	- some examples:
		- duplicate code
		- long methods
		- large classes - God objects
		- feature envy: a class uing the methods of another class excessively
		- dependency on another class's implementation details
		- class that does too little
		- too many literals / magic numbers / magic strings
		- downcasting
		- high cyclomatic complexity
		- too many parameters
		- excessive return of data

- How do you find bad code?
	- Code review
	- Tools
	
- Automatization?
	- SonarCube
	- pmd
	- checkstyle

## 3rd Party Libraries
- check source, documentation, support, thread safety, performance

## REST vs. SOAP
http://spf13.com/post/soap-vs-rest
### REST
- Representational State Transfer
- Sweet spot: exposing CRUD methods
- deals with resources

### SOAP
- Simple Object Access Protocol
- exposes api methods
- transaction support

## NoSql
- non-relational
- distributed
- horizontally scalable (more nodes <-> vertical scaling: more resources to a single node)
- denormalized tables
- key-value stores, document stores, graph stores

## Cohesion vs Coupling
- cohesion: degree to which elements in a module (class) belong together. High cohesion ~ single responsibility
- coupling: degree of interdependence between software modules. Low coupling ~ high reusability

## Composition over Inheritance
- composition allows easier extension of functionality without having to modify interfaces - serves open/closed principle better
- composition avoids the diamond problem (which is a non-issue in Java, since multiple inheritance is not allowed)
- inheritance does not require to explicitly redefine every method in every implementation

## Ajax
- **A**synchronous **J**avascript **a**nd **X**ml
- does not necessarily require xml, requested data can be anything, but structured data is best - json is a common data exchange format
- request data asynchronously
- handle response in callback - usually incorporate into DOM

## Session
- a dialog in networking - multiple requests / responses
- short term permanent
- generally stateful
- useful because
    - http is stateless
    - identifies user on server side - no need to transfer state with every request over the network
    - safer
    - less network traffic

## Continuous Integration vs. Continuous Delivery vs. Continuous Deployment
- CI continuous integration 
    - (incremental) changes on a development branch are frequently merged with a master branch with frequent tests
    - automated unit tests
    - automated build

- Continuous Delivery
    - cont delivery to an environment once the code is ready to ship
    - environment: UAT / Staging / Production
    - delivers to a user base (QA-s or customers)
    - tests include acceptance test - testing business logic
    - small batches of changes fed to proceeding tests

- Continuous Deployment
    - release code to production as soon as it is ready
    - no batching in staging or UAT
    - testing is done before mergin to master on environment similar to production environment
    - production branch is always stable and ready to be deployed in an automated manner

- Tools
    - version control system: Git
    - issue tracking: Jira
    - build System: Bamboo, Jenkins
    - test frameworks: TestNG, JUnit
    - mocking frameworks: Mockito, Easymock
    - tests run automatically as part of build process
    - dependency management: Maven

## Mocking
- mocking overuse: mocking pojos, static services
- simulate behaviour of real object without the actual dependency - allows to test an object without relying on the correctness of its dependencies

## Depenency Injection
### Depenency Injection vs. Inversion of Control
[Martin Fowler](http://martinfowler.com/articles/injection.html)
- Inversion of Control
    - otherwise known as the Hollywood Principle: Don't call us, we'll call you
    - design paradigm in which the we do not know about the concrete implementation of depependecies
    - depend on abstractions not concrete classes
    - this is a stronger statement than "program to interfaces, not implementations"
    - it suggests that instead of high level components depending on low-level components, both depend on abstractions
    - eg.: Instead of PizzaStore depending on concrete Pizza classes, PizzaStore and Concrete Pizza implementations depend on the abstract Pizza Class (Head First Design Patterns)

- Dependency Injection
    - one implementation of IoC
    - dependency is passed to the dependent object to call methods on
    - dependency is created and configured outside of the dependent object

- Other Techniques to adhere to IoC
    - Factory Method Pattern
    - Service Locator

## Design Patterns
- Blueprints of solutions to common problems
- gives common vocabulary
- tried and tested, not necessarily straightforward application of OO principles

### Design Patterns in JDK
[source](http://stackoverflow.com/questions/1673841/examples-of-gof-design-patterns-in-javas-core-libraries)
- Observer pattern: Observable class, wait & notify
- Builder: StringBuilder
- Factory Method: Calendar#getInstance
- Decorator: unmodifiable, concurrent versions of collections; Buffered.../InputStream/OutputStream/Reader/Writer/
- FlyWeight: Integer#valueOf
- Proxy: java.lang.reflect.Proxy
- Chain of Responsibility