# 8470-Secure-web-app-development
This repo contains a digitized version of the course content for IA8470 Secure Web App Development at the University of Nebraska at Omaha.

### Table of contents
[Online discussion area](#online-discussion-area)  
[Location](#location)	 
[Supplies](#supplies)	 
[Projects](#projects)	 
[Class topics](#class-topics)  
[Syllabus](#syllabus)  
[License](#license)  

## Online Discussion Area
I have setup an online discussion board on slack.com for usage in this class. If you decide to work on a group project, I can create some private channels for you to work on, but I want to be able to participate in your conversations - so please use the space on slack. 

Go to [drhale.slack.com](https://drhale.slack.com) and use your unomaha email address to register an account. This will give you access to all of my public slack channels. Use the ia8470-chat and ia8470-questions channels. The chat channel is for general discussions with me or your fellow classmates. The questions channel is for you to ask public questions that I will answer for the whole class. This is better than email if you think that the answer to your question might benefit everyone.

## Location
All classroom activities will take place in PKI room 260 unless otherwise noted ahead of time.

## Supplies

### Hardware/software
* [Virtualbox](http://www.virtualbox.org) or [VMWare Workstation (windows) or Fusion (mac)](/vmware/vmware.pdf)
* [Lucidchart](http://www.lucidchart.com)
* [UNO Cloud Deployment Environments](http://view.unomaha.edu)

### Texts
* Labs: [secwebdev.mlhale.com](http://secwebdev.mlhale.com)
* Ember Textbook (optional): [Rock and Roll with Ember.js](http://balinterdi.com/rock-and-roll-with-emberjs/)
* [Django Documentation](https://www.djangoproject.com/)

## Projects
[Project 1 rubric](/modules/IA8470-project-1-rubric.pdf) - Creating and securing a prototype application
Project 1 will use the skills you learned in Project 1 and have you create some software engineering documents to help you flesh out an idea and implement the client-side portion of the idea that will use for the rest of the semester. For this, and the rest of the projects in the course, you will be able to use your own project idea. You will be expected to satisfy the following rubrics. See the [rubric](/modules/IA8470-project-1-rubric.pdf) for specific details.

## Class Topics
1. Introduction to Web apps ([PDF](/lectures/lecture1.pdf))
 * Threat vectors
 * Basic Architecture
 * Client-side vs Server-side debate
 * [Virtualization primer](/modules/virtualization-primer.md)
 * [Github primer](/modules/github-primer.md)
1. Jump right into Client-side Development ([PDF](/lectures/lecture2.pdf))
 * Model View Controller
 * Ember.js
 * Handlebars
 * AJAX for External API integration
 * [Lab 1](https://htmlpreview.github.io/?https://github.com/MLHale/8470-Secure-web-app-development/blob/master/modules/lab1.html)
1. Develop and harden your own Serverside API ([PDF](/lectures/lecture3.pdf))
 * Intro to Django
 * Server-side Model View Controller
 * Django REST Framework
 * API Hardening
 * [Lab 2](https://htmlpreview.github.io/?https://github.com/MLHale/8470-Secure-web-app-development/blob/master/modules/lab2.html)
1. Step back and think about Software engineering Principles ([PDF Part 1](/lectures/lecture4.pdf))([PDF Part 2](/lectures/lecture4-part2.pdf)) 
 * Software Development Lifecycle
 * Requirements Engineering and Use/Misuse case review
 * Software Architecting
 * Integration patterns between Servers and Clients
 * Browser Object Model Overview
 * JQuery Overview
 * Asynchronicity is your friend
1. Test-driven Development
 * Unit testing
 * Acceptance critera
 * think-test-build-test-repeat
1. Time to be creative
 * Come up with your own project idea or implement something for a friend/family member or community organization
 * [Project 1](#projects): Requirements Engineering, Product Design, Architecture, and Client-side Prototype
 * [Project 2](#projects): Server-side API Creation and Client-side Integration
1. Classwide project final
 * Work together with the rest of your classmates to develop a real app for a non-profit
 * Organization TBA

## Syllabus
### Date/Time: Tuesday 5:30pm – 8:10pm
### Instructor:  Dr. Hale
### Office:  PKI 174-D, (402) 554-3978
### Office Hours:  Thursday 3-6PM, Open door policy, or by appointment
### E-mail:  mlhale@unomaha.edu

### Course Abstract
Web applications are pervasive fixtures of 21st century culture. Web application security is an inclusive, amorphous, term that spans application level security, i.e. ensuring high level code cannot be exploited, server level security, i.e. ensuring server resources such as databases and file systems cannot be exploited, and network security, i.e. ensuring unauthorized parties cannot access a server or tamper with user sessions. The Secure Web Application Development course will mix traditional lecture with hands-on labs to expose students to common web development activities and cross-cut different web application security concepts. After the class, students will be proficient with web application security from the ground up, including the ability to securely deploy and configure a web server (e.g. apache or node.js), select and install a web development framework to best suit application and security requirements (e.g. Django, Ember, or both), and develop secure application architectures and code using at least one web framework. Students will also leave with a basic understanding of core web protocols including TCP/IP, SSL, and HTTP/HTTPs, web development tools (e.g. chrome dev toolkit), unit testing (e.g. QUnit), and best practices for accepting, parsing, and storing user input.

### Grading Breakdown (due dates are tentative)
 * (10%) Flickr Client-side app (Lab 1 - no rubric)
 * (10%) Flickr Server-side app (Lab 2 - no rubric)
 * (30%) Project 1 - Design and Prototype (rubric TBA)
 * (30%) Project 2 - Full Implementation and Documentation (rubric TBA)
 * (20%) Final class-wide Group project (rubric TBA)

Each project will have a specific grading rubric that includes the core requirements for the project (i.e. what the application must do), any required intermediate milestone goals (such as short progress meetings with the instructor), the project due date, and the list of items that must be submitted. Each project will include a presentation component to be presented in class on the project due date. Projects build upon each other. The final Project is considered to be comprehensive. This means that <i>there is no final exam</i>. Final Project presentations will be presented on the day of the Final (December 14th during normal class time).

### Attendance
* Class Attendance: You do not have to attend class except on presentation days (see below). Given the course is one day a week, attendance is highly recommended. Missing a single class is equivalent to missing 2-3 classes of a normal course. If you miss class, you are responsible for getting the material – including any assigned project work.
* Presentation Attendance (Mandatory): If you miss class on a presentation day (tentatively Sep. 28nd, Oct 12th, Nov. 16th, and Dec. 14th) you will receive a 0 on the presentation portion of the project grade unless you have a university-approved excuse or an approved extension (see below). 

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

## What does GPLv3 mean?

[tl;drLegal summary](https://tldrlegal.com/license/gnu-general-public-license-v3-%28gpl-3%29)
