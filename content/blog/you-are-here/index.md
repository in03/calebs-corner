+++
title = "Could I be a Test Engineer?"
description = "Part 1: of a series on getting AI insights on my career path."
date = 2024-12-08

[extra]
banner = "test-engineer.webp"
hot = true

[taxonomies]
tags = ["Career", "Roadmap"]
+++


# Context

This post is the first in a series about forking my career path. I've had a bit of a weird career path, and I've been trying to figure out how to transition into a new role. I've got a lot of broad experience, but also some skillset gaps, depending on the role.

So, as a bit of an experiment, I thought I'd ask an LLM to help.

Given my resume, LinkedIn and GitHub profiles, and a job position description, I asked ChatGPT to make me:

- A scoring framework for career path compatibility.
- A roadmap of skills I should learn to transition into the new role.
- A mermaid diagram of the above roadmap.
- An adjusted resume and cover letter, tailored to the new role.

**For the record**, I don't want ChatGPT to do any "cheating" for me. I'm not doing this with a mind to apply for jobs I *shouldn't*, I'm doing it so I can apply to more jobs I *should*.

{% alert(warning=true) %}
LLMs have a tendency to hallucinate. If you're going to do this, make sure to fact-check the output.
{% end %}

---

## My Background  

I was always interested in computers. 
I got my first one when I was 12, and it was **terrible**. I learnt a lot just by trying to fix it.
It wasn't long before I was running Linux on it, destroying the bootloader, and spending hours on community forums trying to get it booting again.

Fast forward a few years and I got really into film and editing. I loved it in high-school, studied it at uni and got into a video editing position at a startup.

That startup started working on virtual tours, and ended up becoming one of the leading virtual providers in Australia. The process for editing a tour was decidely less creative than a short film or music video. To deal with the tedium I discovered macros and programming. I quickly turned as many of my tasks as possible into automated workflows. That spark led me to software development, then eventually onto IT administration at a high school, where I managed fleets of Apple and Windows devices with tools like Jamf, Intune, and SCCM. Along the way, I picked up CyberSecurity study (still going) and have been diving into the intricacies of keeping systems both secure and functional.

Now, with a growing family and shifting priorities, I’m hunting for roles that let me work remotely, spend more time at home, and—let’s be honest—pay a bit more. While my eclectic background has rounded out my skills and made me a better problem-solver, it’s also left me with gaps that complicate job applications.

So, I had this idea: what if I asked AI to help me bridge those gaps? That’s how this blog series, "You Are Here - AI-Assisted Roadmaps to Alternative Careers," came to life. Each post maps out the skills, tools, and experiences I need to shift into a new role, starting from exactly where I am.

This first post tackles the question: How does someone like me—solid in Python but green in testing—transform into a Test Engineer? Let’s find out.

## Test Engineer 🧪

### Roadmap to Becoming a Test Engineer

#### Phase 1: Foundations of Software Testing

##### What to Learn:
- The basics: unit tests, integration tests, regression tests—what they are and why they matter.
- The Software Development Lifecycle (SDLC) and where testing fits in.
- Python’s unittest module to write basic tests.

##### What to Do:
- Pick a small Python project and write unit tests for each function.
- Try out Test-Driven Development (TDD) for a simple script to get hands-on experience.

#### Phase 2: Tools and Automation

##### What to Learn:
- Testing frameworks like pytest and nose2.
- How CI/CD tools (like GitLab CI/CD) automate the testing process.
- Writing shell scripts to integrate automated testing into workflows.

##### What to Do:
- Set up a GitLab repository with a CI/CD pipeline that runs your tests.
- Write automation scripts for repetitive tasks like deploying tests across environments.

#### Phase 3: Advanced Testing Techniques

##### What to Learn:
- Shift-left testing: catching bugs early in the pipeline.
- Performance and security testing basics.
- Dockerized testing environments for seamless deployment.

##### What to Do:
- Mock APIs in integration tests using Python tools like responses.
- Test a Python-based web app using Selenium or Locust for performance insights.

#### Phase 4: Collaboration and Open Source

##### What to Learn:
- How testing works in collaborative teams (hint: lots of documentation).
- Best practices for quality assurance in large projects.

##### What to Do:
- Contribute tests or bug fixes to an open-source project.
- Add these projects to a portfolio to showcase your testing chops.

### Skill Development Plan

Here’s a mermaid visualisation of what the above might look like in practice.

```mermaid
graph LR
  A[Foundations of Software Testing] --> B[Tools and Automation]
  B --> C[Advanced Testing Techniques]
  C --> D[Collaboration and Open Source]
  A --> E[SDLC & Testing Basics]
  E --> F[Unittest in Python]
  B --> G[CI/CD Tools like GitLab]
  G --> H[Automation Scripts for Test Deployment]
  C --> I[Performance & Security Testing]
  I --> J[Dockerized Testing Environments]
  D --> K[Contribute to Open-Source Projects]
  K --> L[Build Portfolio]
