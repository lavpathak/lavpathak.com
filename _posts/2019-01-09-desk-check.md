---
layout: post
title: Desk Checks - Defect Prevention Technique
---

I have worked on various agile teams following quite a few different techniques. One of the most effective techniques that I have seen to prevent defects is Desk Check. It is a practice used for verification when the developer believes the user story development is done and its ready for testing. The idea here is to catch defects before it reaches the testing phase and minimizes feedback loop.

Participants:
Developer, BA, QA, Other interested parties (Tech lead, Product Owner etc.) 

Time spent:
Should take about 15-20 mins per story

Checklist:
* Does the code satisfy all the acceptance criteria?
* Does the code have proper test coverage? Discuss what coverage is missing so QA can write those (Functional tests etc.)
* Did developer check whether the story may have impacted something else?
* Is the UI interaction consistent with the rest of the app?
* Does it work in all the supported browsers?
* Does the UX match the rest of the app?

I want to mention though that this checklist is not rigid and should change based on the team and project needs. Usually, project leaders (Dev, QA, and Product) come up with this list during one of the earlier iterations and constantly improve it as the project matures.

How does Desk Check help?
* Its the technique to prevent defects and minimize feedback loop
* It also promotes communication between team members with various roles
* When desk check is taking longer than expected then it will reveal other smells such as large stories, missed assumptions, unclear acceptance criteria etc.
* It will help the team maintain coding and UX standards
* It will help non-dev team members get closer to the code 
* As the team will discuss Acceptance criteria over these desk checks it will promote [ubiquitos language](https://martinfowler.com/bliki/UbiquitousLanguage.html)

Hope this post was helpful. In the next post, I will explain another agile practice called Kickoffs that I really find useful as well.

-- Lav