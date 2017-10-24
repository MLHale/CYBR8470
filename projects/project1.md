## CYBR8470 Project Milestone 1: Product Ideation, Design, Mockup, and Prototype
**Assigned: Tuesday 10/24/17**

### Due Date
Tuesday (11/7/17) at 11:59PM

### Total points
315 Points (15% of total grade)

### Overview
For this project you will create a new web application using the skills you have built up over the various labs. The project should be a non-trivial idea that is achievable in 4-5 weeks. You should make something that helps you, someone you know, or an organization that you may be interested in (if you work with a community service organization I will award you up to 10% extra credit).

This first part of the project tasks you with ideation, mockup, and a low-fidelity prototyping activities. By low fidelity, I mean bare bones functionality. You will submit the following as part of this milestone.

1. [Project Summary](#project-summary) - What are you doing and why? How do you install and run the app?
1. [User Stories](#user-stories) - What does your app/product do and who are your users?
1. [Diagrams](#diagrams) - How do the pieces of your application fit together? What does your UI look like?
1. [Prototype](#prototype) - Get started coding

### Project Summary
A project summary should be evocative. It should capture a reader and make them want to contribute to or use your app. In this milestone you will write an executive summary that defines the goals and objectives of your project in language that is easily readable and mental-image evoking. I (or anyone else) should be able to read your executive summary and instantly know a) what the app is and b) why it is important. Summaries should be exciting and interesting. A summary does not need technical detail to describe interesting functionality. You should mention your product by name without using phrases such as "the team", "the class", "the instructor", "project 2" etc.

Your summary will be the first part of your project landing page on Github.

#### Submission materials
You should submit a summary document (in markdown format) on your Github project repo. Call the file README.md and place it in the top-level directory of your Github project.

It should contain the following:

1. The executive summary
1. Installation
1. Getting Started
1. License


#### Example
~~~ markdown
# My Awesome Project app
My awesome app is the best app ever. It helps you do everything so you can be more awesome. In particular, my awesome app has been carefully engineered to leverage leveraging so you can leverage more.

## Installation
```bash
docker-build .
docker-compose run django bash
python manage.py migrate
python manage.py createsuperuser
```

## Getting Started
To run my awesome app simply,
```bash
docker-compose up
```
See in-app menus for help with using specific features.

# License
The MIT License (MIT)

Copyright (c) Matthew L. Hale 2017

insert MIT License text here
~~~

For some good real-world examples, see:
* https://github.com/MLHale/ember-heatmap
* https://github.com/MLHale/gazepoint-websockets
* https://github.com/ember-cli/ember-cli
* https://github.com/facebook/react


#### Grading criteria
Your summaries will be graded as follows following:

| | Meets expectations (9-10) | Some Issues (6-8) | Does not meet expectations (0-5)|
|---|---|---|---|
|Summary statement Clarity| Clearly identifies what your app does and why it is interesting | Some issues, but the summary is passable. | Summary isn't compelling. |
|Summary statement Formatting  | Summary statement is free of formatting errors. | Summary statement has a few grammar, spelling, or structuring issues.  | Summary statement is poorly formatted or contains main spelling or grammar errors. |
|Installation |  Install instructions work. | Some installation steps missing.  | Many installation steps missing.  |
|Getting started   | Outlines several helpful commands to run the app and/or get started using its features.  | Some missing launch instructions.  | No or little direction to start using the app.   |

### User stories
Requirements are extremely important when building a product. Clearly understanding your users and the features that will support them helps you avoid wasting time on tangential ideas that are not core to the product.

In this milestone you will write user stories that define your project requirements.

#### Submission materials
##### The stories
For milestone 1, define the most important user stories for your product. Pay careful attention to structure the user stories as we discussed in lecture. Specifically, submit your user stories using the following template. Include them as markdown in a README.md file located within a folder called `docs` within your main project repository.

As a **user/role**, I want to **goal** so I can **rationale**.

Markdown syntax for this is as follows:
```
As a **user/role**, I want to **goal** so I can **rationale**.
```

*Example 1*: As a **beta tester**, I want to **view the company forums**, so I can **post about my experiences in the beta**.

*Example 2*: As an **administrator**, I want to **moderate the forums**, so I can **prevent abusive behaviors and ensure policy compliance**.

*Example 3*: As an **interested buyer**, I want to **view public releases on the forums**, so I can **track the development process and learn about new product features before they release**.

##### Acceptance criteria.
For each story, define at least one acceptance criteria. The acceptance criteria define the set of requirements that tell you when you are done with the user story.

##### Mis-user stories
In addition to the user stories identify the ways in which users might be able to mis-use your app. Mis-user stories are just like user stories except the user, goal, and rationale are malicious.

##### Mitigation Criteria
For each mis-user story identify at least one `mitigation criteria`. The mitigation criteria define the set of requirements that tell you when you are done protecting against the mis-user story.

#### Grading Criteria

##### User stories
| | Meets expectations (45-50) | Some Issues (35-44) | Does not meet expectations (0-34)|
|---|---|---|---|
|Stated correctly| The user stories identify a user role, goal, and rationale that dictates a user's behavior in the product to be created or assessed. | A few roles, goals, or rationales are missing. | Many issues with role, goal, or rationale identification |
|Acceptance Criteria| The user stories have reasonable, well thought out, acceptance criteria that dictate when the stories are satisfied. | Some criteria are ill defined or ambiguous. Not enough criteria defined. | Many ill-defined or missing criteria.|

##### Mis User stories
| | Meets expectations (45-50) | Some Issues (35-44) | Does not meet expectations (0-34)|
|---|---|---|---|
|Stated correctly| The mis-user stories identify a user role, goal, and rationale that dictates a user's behavior in the product to be created or assessed. | A few roles, goals, or rationales are missing. | Many issues with role, goal, or rationale identification |
|Mitigation Criteria| The mis-user stories have reasonable, well thought out, mitigation criteria that dictate security requirements. | Some criteria are ill defined or ambiguous. Not enough criteria defined. | Many ill-defined or missing criteria.|

### Diagrams
You should never just 'start coding'. Lets think about how your app will be constructed and what it will look like.

##### The first part is a `mockup`
Start by designing a mockup of what your app will look like. This should be an interface design that shows off the different views that a user will see when they interact with your app. You should tie the `mockup` views to specific user stories where possible. You can draw the mockups by hand on a whiteboard or use an image editing or diagraming tool to construct them. The purpose of the mockup is not design aesthetic so much as to understand where you are going. Avoid obsessively editing the mockup. You are looking for **quick and rough** here.

##### The second part is an `architecture diagram`
In addition to the mockup, I want you to create three architecture diagrams using the C4 architecting modeling paradigm discussed in class. See http://static.codingthearchitecture.com/c4.pdf for a quick reference.

You should have a diagram for the first 3 C's - Context, Container, and Component. Again nothing fancy is required here. White board drawings are acceptable - but using a charting tool (or even powerpoint) is better in case your diagrams need to be edited.

#### Submission
You should submit your mockup and architecture diagrams as image files included in the same README.md file in `docs` where your user and misuser stories are located.

#### Grading Criteria
| | Meets expectations (45-50) | Some Issues (35-44) | Does not meet expectations (0-34)|
|---|---|---|---|
|Mockup   |Mockup captures important interface elements that should be present to realize the user stories. It is easy to infer how the app will look when it is completed.   | The mockup captures some information, but leaves a few open questions about how the app will realize the user stories. | The mockup doesn't provide much clarity in terms of how the app will look or function.  |
|Architecture diagrams   | The context, container, and component diagrams are intelligible, appropriate for the application, and elucidate the design of the app.   | One or more of the diagrams is missing some details that would help clarify the app design.  |  Many details are missing - preventing clear understanding of the design.  |

### Prototype
There isn't much formality for this part of the project. I expect you to get started coding. I want to see forward progress towards meeting the specification set forth in your user stories, architecture diagrams, and mockup.

#### Submission
You should submit your github repository project link on Canvas by the due date.

#### Grading Criteria
75 points awarded by frequency of commits and clear evidence of coding effort.

#### License
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">CYBER8470 and related works</span> by <a xmlns:cc="http://creativecommons.org/ns#" href="http://faculty.ist.unomaha.edu/mlhale" property="cc:attributionName" rel="cc:attributionURL">Matt Hale</a> are licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.
