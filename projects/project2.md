# CYBR8470 Project Milestone 2: Implementation and Deployment
**Assigned: Tuesday 10/24/17**

### Due Date
Thursday (12/7/17) at 11:59PM
Final Presentation (12/12/17) at 5:30PM

### Total points
315 Points (25% of total grade)

## Overview
This project tasks you with realizing, through implementation, the design and prototype prepared in Milestone 1.  You will also create core security functionality (if you haven't already), deploy your REST API and client app, and harden them against attack. Hardening your server should be done at the API and container levels to ensure endpoints are properly secured (particularly create and update endpoints).

You will submit the following as part of this milestone.

1. [Implementation](#implementation) - Coding achievements
1. [Hardening](#hardening) - Document the security protections put in place at the container and Application level
1. [Deployment](#deployment) - Creating a code 'release' and deploying your app to where it will live after you are done
1. [Final Presentation](#final-presentation) - The final presentation to be presented on the UNO final time for the class.

# Implementation
In this milestone, the major deliverable is code. All of your code deliverables should be submitted as part of your git repository on Github. Use the same repository that you used in Milestone 1.

## Grading criteria
You will be graded as follows.

### Functionality and Design
| | Meets expectations (33-40) | Some Issues (25-32) | Does not meet expectations (0-24)|
|---|---|---|---|
| Effort and progress| It is clear that the team has made non-trivial effort and progress towards user story realization.| There is some evidence of effort and progress, but more could have been done in the time. | Little effort or progress was made towards user story realization.|
| Documentation| Code artifacts and tasks are documented well. Documentation is clear and illustrative. | Some code is documented, but large portions are not. | Little or no documentation.|
| Best Practices and Don't reinvent the wheel | The product makes good use of design and implementation best practices and libraries of quality code where applicable. | Some best practices are observed, but some code does not follow best practices or reinvents the wheel instead of making effective use of existing resources | Little or no thought of best practices are observed. |
| Design aesthetic | The application user interface and design is high quality. | There are some usability or UI issues, but they do not significantly impede usage. | Design quality is poor to the point of impacting usability. |

# Hardening
In addition to the code artifacts related to user story realization, you should also create security features appropriate to the security posture needed for your app. Your code should include evidence of security functionality in the deliverables.

## Grading criteria
You will be graded as follows.

### Sanitation and Validation
| | Meets expectations (15-20) | Some Issues (10-14) | Does not meet expectations (0-9)|
|---|---|---|---|
| Escapes HTML | All HTML is escaped or rationale is provided as to why raw HTML is needed. Escapes are applied to all html-capable fields (i.e. char, text, etc) | Most HTML is escaped, possibly some issues with escape method leads to possible vulnerabilities.| Raw HTML is not escaped and model fields are vulnerable to POST/PUT attacks.|
| Sanitized Javascript | All fields are sanitized against javascript entry. API calls cannot be used to inject javascript into the database.| Most javascript is sanitized, but there are small issues that may leave the app vulnerable.| Javascript is not sanitized, and can be demonstrably used for attack via POST/PUT.|
| Validator use | Validator are used appropriately for custom validations (such as email, zipcode, or ssn fileds) to ensure fields are well formed. | Most validator use is well founded and applicable, but some obvious needs are missed. | Poor or no use of validators where needed |

### Authentication and Permissions
| | Meets expectations (15-20) | Some Issues (10-14) | Does not meet expectations (0-9)|
|---|---|---|---|
| Session or token-based auth | Application uses session or token auth.| Application uses weak authentication.  | Application does not use authentication |
| Global Permissions | Application uses appropriate permissions for user stories | Application allows permissive access when it shouldn't be permissive. | Application has no or little use of permission checking |
| Super user group | Application includes a super user group or admin group. | N/A | Application does not include a super user group.|
| Application user | App. includes an application user group with appropriate permissions for app usage. | Application user group exists, but has minor permission issues. |No application user group or permissions are completely wrong. |
| Anonymous user | App. includes a user group for anonymous unauthenticated users. Minimal permissions are assigned as appropriate. | App includes a user group for anonymous users, but there are minor permission issues. | No anonymous user group or permissions are completely wrong. |

# Deployment
You are expected to deploy your project to a set of cloud services of your choice. http://surge.sh and https://aws.amazon.com/ or https://cloud.google.com/compute/ are recommended. AWS and Google Cloud provide education credits ($100-200 to students) and are a great option for deployment.

I suggest [Elastic Beanstalk](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create-deploy-python-django.html) or Using [Django on Google](https://cloud.google.com/python/django/) If you use Google and don't want to configure a cloud database, you can use a [Prebuilt Google Compute Django Apache VM](https://console.cloud.google.com/launcher/details/bitnami-launchpad/djangostack?pli=1&project=arklan-custom-ma-1507997989384)

## Grading Criteria
80 points for successfully deploying your app (30 points for client, 50 points for server)
20 points will be given for creating a `tagged release` on a code commit in your Github repository. If you are creating a generic open source library or tool - you might consider adding the release to NPM or another distribution repository.

# Final Presentation
You will be expected to prepare and deliver a final presentation on the class final day. The presentation will be graded using the following rubric.

## Grading Criteria
| | Meets expectations (9-10) | Some Issues (6-8) | Does not meet expectations (0-5)|
|---|---|---|---|
| Enthusiasm and clarity | Presenter exhibits enthusiasm about the work and speaks clearly and confidently. | Some issues with clarity and enthusiasm. | Presenter mumbles, is not clear, and/or does not demonstrate any enthusiasm. |
| Understanding of product | Presenter understands all aspects presented about the project. | Some gaps in understanding when questioned by audience or routinely looks at team slides | Poor understanding of project. |
| Relevant information | Engaging relevant information is presented about the project. | Some valuable information is presented but there are multiple unrelated tangents. | Little valuable information is presented |
| Quick Re-cap of user stories | User stories are re-capped in the context of the implementation. | Too much time is spent reading off user stories or the stories aren't well connected to the results | Way to much time spent or no coverage at all. |
| Quick Re-cap of architecture | Overall architecture is briefly discussed in terms of the developed C4 model. | Too much time is spent on the architecture. | Way too much time is spent or no coverage of architecture. |

| | Meets expectations (17-20) | Some Issues (10-16) | Does not meet expectations (0-9)|
|---|---|---|---|
| Demo* | Deliverable is demonstrated with accompanied explanation of outcomes and success. Product functionality is successfully demoed. | Demo is mildly successful. | Demo fails or is not included in the presentation |
| Results | Results are overviewed, summarized, and relayed to the audience. | Limited results are presented or too much time is spent on minor results. | No results are presented or presenters waste a lot of time on minor results, missing time to present significant outcomes. |

> * Note that Demos should be live demos, but may include videos if the conditions necessary to replicate the demo cannot be re-created in a live in-class demo.

#### License
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">CYBER8470 and related works</span> by <a xmlns:cc="http://creativecommons.org/ns#" href="http://faculty.ist.unomaha.edu/mlhale" property="cc:attributionName" rel="cc:attributionURL">Matt Hale</a> are licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.
