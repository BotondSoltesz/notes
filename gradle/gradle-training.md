# Gradle Training

- Gradle vs Maven
  - Maven: xml
  - Gradle: groovy dsl
  
  - Maven problems
    - project/parent version required
	- no notion of branches
	- parent/child module maintained on both ends of the relationship
	- SNAPSHOT / RELEASE versions
	- no felxibility!
	  - AntRun, ShellScript
	- Poor documentation
	  - stackoverflow, trial & error
	  - some good sonatype books
	- Maven is stagnating, no new versions
	  
  - Gradle solutions
    - group and version is optional
	- parent/child relationship declared in one place: settings.gradle; scriptable
	- Flexibility!
	  - .gradle file is actually groovy
	  - Ant tasks are 1st class citizens
	  - write a plugin - may even be embedded in build file
	- Doc
	  - good userguide
	  - full dsl reference
	  - javadoc/groovydoc
	- Actively developed
	  - ~6 weeks release cycles
	  - vision for future versions
	  
## Workshop
- File names
	- build.gradle
	- settings.gradle (optional)
	- gradle.properties (optional)
- Maven goal ~ Gradle Task
	- doLast {...} - execute when called, not when task is parsed...
- Gradle daemon
	gradle.properties < org.gradle.daemon=true
	or cmd line option: ???
	background process, makes builds faster
- Project DSL in Groovy
	- descriptive like Maven,
	- programmatic like Ant
	- BUT: multiple solutions to one problem... scary at first.
	
- Create tasks:
	1. task <name>; or
	2. task <name>(type:<type>); or
	3. task.create(name:<name>, type:<type>)
	- task types are part of the dsl
	
- Groovy has two types of strings
  - '...' does not have string interpolation
  - "..." has string interpolation - $variableName, substituted from .properties (gradle.properties in user home or local dir), or command line -PvariableName=fooBar
  
- property resolution order
	- system properties: -D... - not recommended
	- project properties: -P...
	- GRADLE_USER_HOME
	- project dir
	- project root dir
	
- Avoid dot separated notation in strings - used for property resolution; use camle case instead

### Gradle Wrapper
- Command Line Tool
- gradle wrapper
	- generates gradle dir
	- gradle-wrapper.jar, gradle-wrapper.properties
	- gradlew.sh
	- gradlew.bat
- wrapper downloads gradle, isntalls, runs
- standard entry point for project, even in spite of gradle installation
- can specify own gradle distribution in wrapper properties
- recommended for multi-user projects
- gradlew scripts should be under version control

### Standard java project
- apply plugin: 'java' in build.gradle
	- plugin adds java specific tasks to gradle
	- gradle build
	- other goals: compile, runtime, testCompile, testRuntime
	- build, clean

- apply plugin: 'application' -> install, run tasks
	- gradle run

### Testing
- JUnit is default

### Multi Project
- settings.gradle is mandatory
- describes modules
- project name by default is the parent folder - meh... 
	- declare it in settings as
	- rootProject.name = ...
- root build.gradle	

### Dependencies
- binary artifact handling
- configurations: similar to Maven Scopes
- dependency declarations

		repositories {
			jcenter();
		}
		
		configurations {
			testCompile
		}
		
		dependencies {
			testCompile group: 'junit', name: 'junit', version: '4.10'
			or
			testCompile 'juint:junit:4.10'
		}
		
- on the command line camel case abbreviations are allowed 

# Session 2
Workshop - convert mvn based game of life to groovy
github.com/wakaleo/game-of-life

| cmd | build time |
| --- | --- |
| mvn clean install -U | 14.25 |
| mvn clean install -DskipTests | 8.13 |

## gradle init
- init empty project in empty dir
- attempt to convert maven project to gradle

## cmd line options
- `-x <task>`: exclude task
- `-a`: no-rebuild; builds only current sub-module
- `-daemon`
- `-parallel`
- `-configure-on-demand`: used mainly by IDEs; only recommended when gradle used in a declarative manner

## config options
- project name by default is project dir, but can be treated independently
- settings.gradle > include ':project-name', project('project-name').projectDir=...

## Common Plugins
### Coverage
- apply plugin: 'jacoco'
- run tests
	java plugin has task 'test' - look up documentation...
	
		test {
			include '**/When*.class'
			include '**/*Test.class'
			exclude '**/integration/**/*'
		}
	NB! .class instead of .java!

- dependsOn:
	- flexible, may depend on almost anything...
	- task, fileset, ...
	- executes task depended on
	
- rerunning tests
	- gradle check is not enough
	- gradle --rerun-tasks check

### FindBugs (sourceSets really...)
- gradle operates with **sourceSets**
- any number of sourceSets can be defined
	- default: main, test
- findbugs does not care if a source set is main or test; defines a task for every sourceSet
- e.g.: on Android: combinations of sourceSets -> flavours (handled by the Android plugin)


## Todo! copy diffs from https://github.com/kicsikrumpli/gradle-game-of-life/commits/convert_to_gradle