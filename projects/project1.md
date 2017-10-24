## CYBR8470 Project Milestone 1: Product Ideation and Mockup
**Assigned: Tuesday 10/24/17**

### Due Date
Tuesday (11/7/17) at 11:59PM

### Overview
For this project you will create a new web application using the skills you have built up over the various labs. The project should be a non-trivial idea that is achievable in 4-5 weeks. You should make something that helps you, someone you know, or an organization that you may be interested in (if you work with a community service organization I will award you up to 10% extra credit).

This first part of the project tasks you with ideation, mockup, and a low-fidelity prototyping activities. By low fidelity, I mean bare bones functionality. You will submit the following as part of this milestone.

1. [Project Summary](#project-summary) - What are you doing and why? How do you install and run the app?
1. [User Stories](#user-stories) - What does your app/product do and who are your users?
1. [Application Architecture](#application-architecture) - How do the pieces of your application fit together?
1. [UI Mockup](#ui-mockup) - What will your app look like?
1. [Prototype](#prototype)

### Project Summary
An executive summary should be evocative. It should capture a reader and make them want to contribute to or use your app. In this milestone you will write an exective summary that defines the goals and objectives of your project in language that is easily readable and mental-image evoking. I (or anyone else) should be able to read your executive summary and instantly know a) what the app is and b) why it is important. Executive summaries should be exciting and interesting. It is the first (and likely the only) chance for you to engage your reader and, in a real world setting, would determine if your product gets funded. An executive summary does not need technical detail to describe interesting functionality. You should mention your product by name without using phrases such as "the team", "the class", "the instructor", "project 2" etc.

Your executive summaries will be the first part of your project landing page on github.

#### Submission materials
You should submit a summary document (in markdown format) on your github project repo. Call the file README.md and place it in the top-level directory of your github project.

It should contain the following:

1. The executive summary
1. Installation
1. Getting Started
1. Running the app
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

### Grading criteria
Your summaries will be graded as follows following:

| | Meets expectations (9-10) | Some Issues (6-8) | Does not meet expectations (0-5)|
|---|---|---|---|
|Problem statement| Clearly identifies the context of the problem addressed by your project. Answers the question - Why is this an area of interest? | Problem context not clearly identified, leaves reader wondering why the project is necessary. | Problem context unclear or not identified. |
|Project goals| Project goals are clear, concise, and in a bulleted list. Efforts are clearly tied to addressing the identified problem. Goals are stated at a high level and are free of technical jargon or uncessary detail. | Goals are not as clear, 'in the weeds,' or not directly applicable to the problem. | No goals are identified or there are severe clarity issues. |
|Project Merit | The benefits for pursing the project are clear and tied to addressing the identified problem(s). End user, societal, and industrial benefits are stated - if relevant.| Benefits and merits of the proposed work are muddled or not tied to the problem statement. | Merits are difficult to identify or missing entirely. |
|Length| Summary is of reasonable length (no longer than 1 page, not too short)| Summary is slightly too short or too long | Summary is extremely short or long. |
|Grammar, spelling, syntax | Summary is evocative and free of grammar, spelling, or syntax issues. | Summary contains a few syntax, grammar, or spelling issues. | Summary is difficult to read due to glaring grammar, syntax, or spelling issues|
|Use of markdown| Summary should render as the project homepage on your github repo. It should following markdown conventions. | Doesn't perfectly follow markdown syntax or isn't rendered on the repo home page correctly. | Doesn't use markdown or isn't displayed in the repo.|

### Proposed project timeline
An important part of project planning is identifying tasks to be completed to address project goals and arranging those tasks in such a way that they can be completed within a (typically) fixed interval of time. In this class, that interval is the length of the semester. For milestone one, prepare an overall project timeline that identifies large tasks to be completed, estimates time of completion, and arranges those tasks chronologically over the project lifespan.

#### Submission materials
You should create a gantt chart and identify *major* tasks that span the rest of the semester. I suggest using a gantt chart creation tool such as [ganttpro](https://ganttpro.com) - but you may feel free to create one in a tool of your choice. Once your chart is created, save it as an image file and display it in your README.md file, beneath your executive summary on your github repo.

#### Grading Criteria
Your gantt chart will be graded as follows:

| | Meets expectations (9-10) | Some Issues (6-8) | Does not meet expectations (0-5)|
|---|---|---|---|
| Identifies major tasks | Major tasks addressing your objectives (from the executive summary) are identified by name. Set of tasks is appropriate to the objectives and time frame. | The set of tasks is not scoped well (too much or too little) or they do not seem to address the objectives well. | No or few tasks are identified - or they are extremely poorly scoped. |
| Chronologically arranged | Tasks are intelligently arrayed by time. | Tasks are poorly arranged. | Tasks are extremely poorly or not arranged in time. |
| Achievable | Timeline is expectedly achievable given time frame and team size. | Timeline/task achievability is questionable. | Timeline is completely unrealistic.
| Displayed in readme file| Your gantt chart is accessible in your github project repo on the same page as the executive summary | N/A | Your gantt chart is not available in your github repo |

### Risk list
As you will probably discover, there are many things that can go wrong when working on a project. Issues with skillsets, technology, team member availability, etc, may arise as the project goes forward. As we discussed in class, there are three options available to deal with project management risks: monitor it (Watch it), mitigate it (do something about it), accept it (give up). For milestone one, you should identify the major sources of risk and what you plan to do about them. Prepare a risk list and rank them by risk level using the standard formula, i.e. impact x likelihood, to describe the top 5 risks that might affect your team's ability to pull off the tasks identified in your timeline and dictated by your objectives.

#### Submission materials
You should prepare a risk list table and add it to your README.md file in your github project repo, beneath your Gantt chart. The table should use markdown syntax such as:

```markdown
|Risk name (value)  | Impact     | Likelihood | Description |
|-------------------|------------|------------|-------------|
|Some risk (40) | 8 | 5 | Some description  |
```

to generate github-displayable tables such as:

|Risk name  | Impact     | Likelihood | Description |
|-----------|------------|------------|-------------|
|Some risk (40) | 8 | 5 | Some description  |

#### Grading Criteria
Your risk list will be graded as follows:

| | Meets expectations (9-10) | Some Issues (6-8) | Does not meet expectations (0-5)|
|---|---|---|---|
|Impact| Each risk has an identified impact factor. Factor should be based on how significant the event would be towards the disruption of your project activities. | Some factors missing or unreasonable. | No factors identified or factors are completely unreasonable |
|Likelihood | Each risk has an identified likelihood factor. Factor is reasonable. | Some factors missing or unreasonable | No factors identified or factors are completely unreasonable |
|Name and description| Risk name and description are appropriate and understandable. | Some risks are unclear. | Most or all risk are unclearly described or descriptions are missing. |
|Coverage and relevance | Identified risks are relevant to the project and cover obvious potential problem areas. | Some coverage or relevance is missing. | Risks do not address obvious potential problem areas or aren't relevant.

### Application requirements
Requirements are extremely important for conducting a successful project - of either the creative or assessment-oriented varieties. Collecting requirements about an application means understand what your end user (or what the end user of a product you are assessing) is going to do with the product. To understand requirements we often define user stories and use cases to encapsulate and represent behavior.

#### Submission materials
##### User stories
For milestone 1, you should define the 5 most important user stories for your product (or the product you are assessing). Pay careful attention to structure the user stories as we discussed in lecture. Specifically, submit your user stories using the following template. Include them directly on, or as a link from, your github repo README.md file.

As a **user/role**, I want to **goal** so I can **rationale**.

Markdown syntax for this is as follows:
```
As a **user/role**, I want to **goal** so I can **rationale**.
```

For each user story, also define acceptance criteria.
* For *'maker'* projects your acceptance criteria should define the set of things required to understand when you are done with the user story.
* For *'breaker'* projects, your acceptance criteria should define when you are done testing to ensure that the user story is met by the application securely.

##### Use and mis-use case diagram
Also for milestone 1, you should create a use case diagram that identifies the most relevant use cases in your application and how those use cases might be exploited with mis-uses. If you are a maker team, the use case diagram should lay out the relevant collection of uses you will need to support in your product. For breaker teams, the use case diagram should illustrate the relevant uses and mis-uses that you will be exploring in your evaluation. Ideally these should reflect the original product goals of the app/product you are assessing.

Use Lucid chart to create your use/mis-use case diagram. Share the lucid chart and link to it from your github README.md page in your repo.

#### Grading Criteria
User stories and use cases will be graded as follows.

##### User stories
|Each user story | Meets expectations (9-10) | Some Issues (6-8) | Does not meet expectations (0-5)|
|---|---|---|---|
|Stated correctly| The user story identifies a user role, goal, and rationale that dictates a user behavior in the product to be created or assessed. | A role, goal, or rationale is missing. | Many issues with role, goal, or rationale identification |
|Acceptance Criteria| The user story has a reasonable, well thought out, set of acceptance criteria that dictate when the story is satisfied. Breakers should identify in their acceptance criteria when they know they are done evaluating the user story. | Some criteria are ill defined or ambiguous. Not enough criteria defined. | Many ill-defined or missing criteria.|

##### Use / Misuse case diagram
|| Meets expectations (27-30) | Some Issues (20-26) | Does not meet expectations (0-19)|
|---|---|---|---|
|Coverage| Use case diagram covers expected user interactions with the product. Efforts are clear to identify use cases that fit into identified user stories. | Some missing use cases. | Many missing use cases in the diagram.|
|Misuse cases| Diagram includes relevant coverage of misuse cases. | Some missing misuse cases in the diagram. | Many missing misuse cases in the diagram. |
|Use of stereotypes/arrows| Diagram correctly and appropriately uses stereotypes (e.g. extends, includes) and arrows to connect diagram elements. | Some incorrectly used diagram features. | Many issues with usage of stereotypes or arrows in the diagram. |

### Resources Needed
Every team needs *something* to pull off their project. Clearly identify the technologies and products involved in your project.

#### Submission materials
Under your requirements section in the README.md file in your github repo, clearly identify the technologies, products, and languages involved in your project. Include a table that identifies which team member will investigate each needed resource. Also include a column indicating whether or not material resources are needed from Dr. Hale.

Use the following table structure.
```markdown
|Resource  | Dr. Hale needed? | Investigating Team member | Description |
|-------------------|---------|---------------------------|-------------|
|Some resource| No | Bob | Some description  |
|e.g. PLC unit | Yes | Gary | A programmable logic controller from siemens for investigation.|
```

This will generate a table of the form:

|Resource  | Dr. Hale needed? | Investigating Team member | Description |
|-------------------|---------|---------------------------|-------------|
|Some resource| No | Bob | Some description  |
|e.g. PLC unit | Yes | Gary | A programmable logic controller from siemens for investigation.|

#### Grading Criteria
You will be graded as follows:

|| Meets expectations (9-10) | Some Issues (6-8) | Does not meet expectations (0-5)|
|---|---|---|---|
|Coverage| A reasonable set of resources are listed that are appropriate to your project.|Some missing critical resources that are needed but not listed. | Many critical resources are needed, but not listed.|
|Conforms to structure| Entries follow tabular format specified. Each resource has an assigned team member investigating it. | Some issues following structure. | Table not formatted or many issues.|

### First Sprint Plan
This milestone represents the first step forward in your capstone project. As part of that effort, you need to plan your first real sprint.

#### Submission materials
This part of the milestone is strictly captured by Trello. Make sure Dr. Hale has been added to your project Kanban boards. In your Kanban board, create a board following the structure discussed in Lecture 4. Use this structure to identify the tasks that you will tackle in your first sprint. Put those tasks in the **Sprint To-do** category.

#### Grading Criteria
No fancy grading rubric here. You will receive points for participating (as measured by the activity flow) in Trello.

### Teamwork
Lastly, it is important that you realize you are working on a TEAM (except for the few of you that are soloing). I will say now, that your grade is dependant on your participation. I will not allow individuals to sluff off on a team. Your grade is therefore based partly on a *participation factor*. Essentially if you dont participate (and trust me I will know) - you will not get all of the team points on your project grade.

Your final grade on the milestone is computed as follows:
**team_grade** * **participation_factor** = **individual_grade**

**participation_factor** is a number from .4 to 1 that is based on four measured participation factors:

1. Contribution to the github repo
1. Activity in the slack channel for your project
1. Activity on Trello
1. Team evaluations

If you do your work and participate, you will get the full team poitns (**participation_factor**=1), if you don't, you will get as little as 40% of the team points. This means that you can **FAIL** even if your team succeeds. - So don't sluff off.

#### License
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">CYBER4580 and related works</span> by <a xmlns:cc="http://creativecommons.org/ns#" href="http://faculty.ist.unomaha.edu/mlhale" property="cc:attributionName" rel="cc:attributionURL">Matt Hale</a> are licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.
