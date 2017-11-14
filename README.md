### Table of contents
[Online discussion area](#online-discussion-area)  
[Location](#location)  
[Supplies](#supplies)  
[Class topics](#class-topics)  
[Syllabus](#syllabus)  
[License](#license)  

## Viewing these materials
The class materials are best viewed at [https://mlhale.github.io/CYBR8470/](https://mlhale.github.io/CYBR8470/)

## Online Discussion Area
I have setup an online discussion board on slack.com for usage in this class. If you decide to work on a group project, I can create some private channels for you to work on, but I want to be able to participate in your conversations - so please use the space on slack.

Go to [drhale8470.slack.com](https://drhale8470.slack.com) and use your unomaha email address to register an account. Alternatively, you can use [this link](https://join.slack.com/t/drhale8470/shared_invite/MjI5MTIzNzg1NjgyLTE1MDMyNTExNTUtZDFlY2NjZGZkOA).This will give you access to the class slack channel. The chat channel is for general discussions with me or your fellow classmates. The questions channel is for you to ask public questions that I will answer for the whole class. This is better than email if you think that the answer to your question might benefit everyone. You can also send me private messages. Generally I am faster at replying on slack than I am by email.

## Location
All classroom activities will take place in PKI room 260 unless otherwise noted ahead of time.

## Supplies
* Laptop with the ability to run Docker and a text editor
* Generally this means 6-8+ GB Ram, modern processor

### Hardware/software
* [Docker](https://www.docker.com/get-docker)
* [Lucidchart](http://www.lucidchart.com)

### Texts
* Ember Textbook (optional): [Rock and Roll with Ember.js](http://balinterdi.com/rock-and-roll-with-emberjs/)
* [Django Documentation](https://www.djangoproject.com/)

## Lab Listing (links also in Class topics)
* [Github primer](/modules/github/README.md)
* [REST and Twitter API Exploration](/modules/restful-api/README.md)
* [Container primer](/modules/containers/README.md)
* [Creating a new Web service](/modules/building-a-server/README.md)

## Class Topics
* Class Intro ([Lecture 1 Slides](/slides/lecture1/index.html))
  * Overview of Web apps
  * Concept inventory
  * Threat vectors
  * HTTP Review
  * [REST and Twitter API Exploration](/modules/restful-api/README.md)
  * [Github primer](/modules/github/README.md)
  * [Container primer](/modules/containers/README.md)
* Web Services ([Lecture 2 Slides](/slides/lecture2/index.html))
  * Background
  * Service oriented Architectures (SOA)
  * Web Service Standards (`WS-*`)
  * Methods: SOAP / REST / Web sockets
  * Data Formats: XML, JSON
* Server-side Development in Django ([Lecture 3 Slides](/slides/lecture3/index.html))
  * Intro to Django
  * Server-side Model View Controller Pattern
  * Django REST Framework
  * Django in Docker
  * [Creating a new Web service](/modules/building-a-server/README.md)
  * [Building API endpoint exercise](/modules/building-a-server/django-exercise.md)
* Test-driven Development ([Lecture 4 slides](./slides/test-driven-development.pdf))
  * Unit testing
  * [Pen testing Lab](/modules/penetration-testing/README.md)
  * Acceptance critera
  * think-test-build-test-repeat
* Client-side Development
  * Browser Object Model Overview ([Lecture 5 Slides](/slides/intro-to-clientside-lecture.pdf))([Lecture 6 Slides](/slides/lecture6/index.html))
  * JQuery Overview
  * [MIT jQuery Lab](http://web.mit.edu/6.813/www/sp16/labs/lab2-javascript-jquery/)
  * [Optional Codeacademy HTML/CSS Labs](https://www.codecademy.com/tracks/web)
  * [Optional Mozilla Developer Network Tutorials on HTML/CSS](https://developer.mozilla.org/en-US/docs/Web)
  * [Optional Mozilla Developer Network tutorials on Javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
  * [Optional Javascript exercises](https://www.w3resource.com/javascript-exercises/javascript-basic-exercises.php)
  * Asynchronicity is your friend
  * Model View Controller
  * Ember.js
  * [Ember.js lab](https://guides.emberjs.com/v2.16.0/tutorial/ember-cli/)
  * Handlebars
  * AJAX for External API integration
* Software Architecture
  * Reading: [https://leanpub.com/visualising-software-architecture/read](https://leanpub.com/visualising-software-architecture/read)
  * C4 MetaModel and Description: [https://c4model.com/](https://c4model.com/)
* Time to be creative
  * Come up with your own project idea or implement something for a friend/family member or community organization
  * [Project Milestone 1](/projects/project1.md) - Product Ideation, Design, Mockup, and Prototype
  * [Project Milestone 2](/projects/project2.md) - Implementation and Final Delivery
* Configuration and hardening
  * Deployment
  * Hardening
  * [IP Tables and OS Hardening](modules/iptable-exercise.md)
* Finishing up
  * Project Milestone 2 - TBA
  * Class-wide Final Project - TBA

## 2016 Archived Lab content
[secwebdev.mlhale.com](http://secwebdev.mlhale.com)

## 2016 Projects and Final Presentations:
* MHefley (PEViewer - Private REPO)
* SFrench (FlightPlanCompanion - Private REPO)
* DEllis and GFilter ([SimpleFit](https://github.com/dlellis/SimpleFit))
* Fkangaye (Micro - Private REPO)
* IShrestha ([Karyalaya](https://github.com/IsaacShrestha/Final-Karyalaya-Project))
* GWethor ([Chartreuse](https://github.com/gewethor/Chartreuse-Front-End))
* Lanreogunmola and SBagayogo ([Afrimat](https://github.com/lanreogunmola/Afrimat))
* DNelson ([Waze Place Discovery and Audit](https://github.com/DaJoNel/wpda))

## Syllabus
**Date/Time**: Tuesday 5:30pm – 8:10pm
**Instructor**:  Dr. Hale
**Office**:  PKI 174-D, `(402) 554-3978`
**Office Hours**:  Open door policy, or by appointment
**E-mail**:  `mlhale@unomaha.edu`


### Course Abstract
Web applications are pervasive fixtures of 21st century culture. Web application security is an inclusive, amorphous, term that spans application level security, i.e. ensuring high level code cannot be exploited, server level security, i.e. ensuring server resources such as databases and file systems cannot be exploited, and network security, i.e. ensuring unauthorized parties cannot access a server or tamper with user sessions. The Secure Web Application Development course will mix traditional lecture with hands-on labs to expose students to common web development activities and cross-cut different web application security concepts. After the class, students will be proficient with web application security from the ground up, including the ability to securely deploy and configure a web server (e.g. apache or node.js), select and install a web development framework to best suit application and security requirements (e.g. Django, Ember, or both), and develop secure application architectures and code using at least one web framework. Students will also leave with a basic understanding of core web protocols including TCP/IP, SSL, and HTTP/HTTPs, web development tools (e.g. chrome dev toolkit), unit testing (e.g. QUnit), and best practices for accepting, parsing, and storing user input.

### Grading Breakdown
 * (30%) Labs
   - Twitter REST API (Lab 1 - no rubric)
   - Containers (Lab 2 - no rubric)
   - Building a Server (Lab 3 - no rubric)
   - Penetration Testing (Lab 4 - no rubric)
   - Javascript (Lab 5 - no rubric)
   - Ember (Lab 6 - no rubric)
 * (15%) Project Milestone 1 - Product Ideation, Design, Mockup, and Prototype
 * (25%) Project Milestone 2 - Implementation and Documentation
 * (20%) Class Wide Community Service Project
 * (10%) Class Participation

Each project will have a specific grading rubric that includes the core requirements for the project (i.e. what the application must do), any required intermediate milestone goals (such as short progress meetings with the instructor), the project due date, and the list of items that must be submitted. Each project will include a presentation component to be presented in class on the project due date. Projects build upon each other. The final Project is considered to be comprehensive. This means that *there is no final exam*. Final Project presentations will be presented on the day of the Final (December 14th during normal class time).

### Attendance
* Class Attendance: You do not have to attend class except on presentation days (see below). Given the course is one day a week, attendance is highly recommended. Missing a single class is equivalent to missing 2-3 classes of a normal course. If you miss class, you are responsible for getting the material – including any assigned project work.
* Presentation Attendance (Mandatory): If you miss class on a presentation day (Dates TBA) you will receive a 0 on the presentation portion of the project grade unless you have a university-approved excuse or an approved extension (see below).
* Part of your grade is based on class participation - which includes being present and actively engaged in class.

### Group Work
Students may choose (or be compelled) to work in groups. Group projects will include an individual participation grade worth 40% of the total group points, e.g. a group may make a 100% on a particular project, but an individual with low participation in the group may make a 60%. Participation will be anonymously rated by other group team members and the instructor.

### Team formation
The instructor reserves the right to make a change to any team or any project during the course of the semester for any reason that may or may not be disclosed. Project rescoping will be performed in this event.

### Service Learning / Real World Customers
As part of UNO’s strategic initiatives, individuals or groups may be partnered with community organizations in Omaha for service learning through the center for community engagement. If community partners can be identified, student projects (group or individual) in the class will develop secure web applications for meeting community needs. In the event of community projects, appropriate scoping will be considered to ensure that community needs can be met within the time constraints of the coursework.

### Project Extensions and Late work
Sometimes unforeseen events occur or development takes longer than expected. In such cases, project extensions will be allowed. To receive a project extension, individuals or groups must request an extension at least 24hours in advance of the project due date. Extension time frames are at the discretion of the instructor, but generally will not be longer than 1 week. Failure to request an extension 24 hours prior to the due date means that the work is due at the specified time. Late work without a requested extension will receive a 5% point reduction per day up to a total of 40%. Late work submitted 2 weeks after an original (or extended) due date will not be accepted.

### Special accommodations for students with disabilities
Students with disabilities requiring special accommodations must contact disability services. Disability services may be reached by phone at (402) 554-2872 or by email at unodisability@unomaha.edu.

### Special accommodations for active duty or reserve military
Students serving in the military requiring special accommodations (e.g. unit deployment) must contact the office of Military and Veteran Services by phone at (402) 554-2349 or by email at unovets@unomaha.edu.

### Plagiarism
The university policies on cheating and plagiarism apply in this course. Except on designated group work, the expectation is that every student will do their own work. Students under suspicion of plagiarism for individual assignment submitted materials will be given an opportunity to defend themselves. If after defense the instructor still believes the work to be plagiarized the department chair will be notified and the grade evaluation for the assignment will be lowered to a value between 50% and 0% at the discretion of the instructor. If a second occurrence of plagiarism occurs, the student will receive an F for the course and the registrar’s office will be notified that the student is not permitted to withdraw from the course. In addition the department chair and dean will be notified.

### Work Retainment
The CS and IS programs in the College of IS&T are accredited through ABET (the Accreditation Board for Engineering and Technology. This organization occasionally requires that we keep samples of student work.

The instructor may retain a copy of your exams (with names and any other identifying information removed) for accreditation or pedagogy purposes, unless you specify otherwise in writing.

In addition, the instructor retains the right to use any code or project artifacts developed in the course for pedagogy, research, or service learning purposes. Student web project code developed in the course may be used in future secure project development courses, by the instructor for research purposes, or by designated stakeholders.

## License
Secure Web App Development
Copyright (C) 2016  Dr. Matthew L. Hale

### Lesson content
Copyright (C) [Dr. Matthew Hale](http://faculty.ist.unomaha.edu/mhale/) 2017.

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">This lesson</span> is licensed by the author under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.

### Software affiliated with the course
This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.

#### What does GPLv3 mean?
[tl;drLegal summary](https://tldrlegal.com/license/gnu-general-public-license-v3-%28gpl-3%29)
