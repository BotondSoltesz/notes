# Introduction
Dear Committee Head, Dear Experts,

My name is Daniel David Kovacs. Please allow me to briefly introduce myself.

## A little bit about my background
I have been trained both as an architect and as software engineer; although it may seem like a minute detail, I think this fact does deeply influence my thought process.

## About my recent role
This is my third year at Epam as software engineer. After a few in/house projects I have become a member of the Hotels.com project focusing on backend services for mobile applications.

### Technological achievements
I am particularly proud of my contribution to our team's mobile api service. To give a broad overview its role is to provide edge services to mobile applications via thin stateless Rest services with the occasional facade-like aggregate logic. I have taken part in the creation of a multi module maven archetype, whose latest iteration now serves as an architectural foundation for upcoming mobile services projects.

As part of this archetype there is a build time integration test in place. I consider this my brain child, and I am lucky to say that it has proven itself useful a number of times. In essense it deploys the webapp in a separate jvm instance and the test methods perform actual http requests against the webapp. A particularly interesting problem that had to be solved stemmed from the fact that we are required to integrate componenets outside of our ownership that sit at the seams of our code. Despite the regular Spring profile mechanism being ommitted from these components I have managed to create a systematic method for mocking them to enable efficient integration testing.

This also serves the basis of our currently formulating testing strategy in CI environment.

I am also proud of the structure I have created for some of the team's plugin projects. Due to rather volatile specifications and requirements they quite frequently had to endure alterations. I am happy to say that in most of such cases it was sufficient to make changes in configuration only, or occasionally had to add only a minimal amount of code. To me this demonstrates that I have managed to create a level of abstraction and loose coupling that is a good match to its evolving environment allthewhile not hindering the ease of understanding the project in its entirety.

The most current project I am involved with is the cross device shortlist service. This is developed within a subteam in which I have been tasked to perform a lead role. Technical challanges include

- the early adoption of Docker on the project. I have successfully the operational details, and put together a quickstart guide for the project; I  have also given same hands on help to our QA-s in integrating this technology into their testing workflow.
- further challange has been to update the project structure generated from the archetype to fit the needs of this and the upcoming projects.

### My roles in mentoring and leadership
I find my mentoring and leading role equally important on this new project, which has proven itself to be a fertile ground for ramping up the juniors with real world problems. My tasks beside development have been

- Breaking down the project into stories that are generous enough to allow the juniors to work on them with a reasonable amount of independece,
- yet create a structure that guides them in tackling the inherent complexity; 
- Demonstrate the progress on weekly demos to the client: my approach has been to give a broad overview of the project and hand over the stage to the respective author of a particular component to demonstrate; this has proven itself to be welcome structure
- communicate with the owners of the backend service we consume to synchronize between the finer details of the business requirements and the api available to us for consumption.
 
I am happy to say that the story breakdown of the shortlist service is a valueable blueprint for similar upcoming stories, such as the opt-in migration.

#### whiteboard sessions
To me leadership also means being able to create a productive environment that empowers the growth of a project. For this reason I consider it a personal achievement that I successfully layed the foundations for what we came to know as whiteboard sessions. It came from the realization that some ideas, especially abstract ones are easier to communicate visually. This has proven to be an effective tool within our team both at the conceptional phase of new projects as well as understanding existing legacy structures. Results of such sessions have also proven to be effective tools in communicating with technical stakeholders of the client as well as with third party partners.

Besides personal means of knowledge transfer I place great value on self-sufficient learning; to enable this I actively advocate thorough documentation, and make it a point to take notes whenever I come accross something new and interesting.

### Self improvement
I place great importance on self improvement by reading technical books and articles. My interests have a broad spectrum. I make it a point to also read materials that feed directly into my daily work. Recent examples include reading up on Docker which helped with the dockerization of shortlist edge service. A further example is Maven, as configuring the integration tests for a multi-module project requires somewhat deeper understanding of its internals than what is required for the everyday dependency management tasks.

## Why I think I am a viable candidate for D3 level
To my mind - on the technical side - it is not enough for a level 3 developer to have an appropriate skills, but also an inner drive to constantly enhance their knowledge. Even more importantly they should readily share their knowledge both in the form of trainings and casual shop-talk. I feel that I have demonstrated this in my day to day work as well as in my participation of the juniors' ramp up process.

As for people-skills It is essential to be able to synchronize the work of multiple people. I belive I have demonstrated my ability to do this by coordinating the tasks of the juniors as well as being a contact point in correspondence for both the client and the team whose backend service we depend on.

I hope I made a convincing case and I am looking forward to a thrilling conversation.

