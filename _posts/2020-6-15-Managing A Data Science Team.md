---
title: "Managing A Data Science Team"
categories:
  - team management
tags:
  - management
  - data science
--- 

In this post, I will share my data science manager experience.  

![_config.yml]({{ site.baseurl }}/images//Managing A Data Science Team/management.png)




Here is our list of 12 signs the company you are interviewing with for a data scientist job should be avoided (and the questions to ask during the interview). The first six mainly apply to companies that already have multiple data scientists or analysts. If you’re thinking of joining a company as their first data scientist, you’ll face a whole different set of challenges, including most likely doing a lot of data engineering work (see flag 1) and spreading a data science mindset. Someone has to do it, but we generally advise against it for your first data science role unless you come from an engineering background and want to do that work. If there’s just a data science leader and they’re building out a team, ask how they plan to handle the issues raised below, but keep in mind it’s always easier to promise an ideal system than implement one.
Red flags on how the data science team runs
1. No data engineering or infrastructure.
Data science requires data to be easily available for analysis. If the company doesn’t have a well-maintained data infrastructure, you won’t have what you need to do your job. A data engineer is a person who prepares data for analysis, and if your company doesn’t have them you’ll have to do the work yourself. If you feel qualified to take on the role of a data engineer that may be okay, but otherwise you’ll be struggling to deliver anything of value.
Question to ask during the interview: what is your data infrastructure like and who maintains it? What format is the data typically in (Excel, a SQL database, csv)?
2. No peer review between data scientists.
A strong data science team will have ways to ensure mistakes don’t slip through the cracks. These can include code reviews, practice presentations, and consistent check-ins with the team. If the team doesn’t consistently do these, mistakes won’t be found until the work is already delivered, which usually ends with someone getting reprimanded.
Question to ask: what steps does the team take for QA and peer review?
3. No standard set of languages on the team.
Many data science teams take the approach of letting anyone on the team use any language they want. The idea is that if everyone uses their favorite languages work will be completed faster. There is a huge problem with this: when everyone uses separate languages, no one will be able to pass off work to anyone else. Every data science task will have a single person responsible for it, and if they quit, get sick, or just need help no one will be able to do so, creating a very stressful environment. It’s fine to use R, Python, or even dare we say SAS, but just have a consistent set of languages amongst the team.
Question to ask: what languages does your team use, and how do you decide whether to adopt a new one?
4. They don’t understand the data hierarchy of needs.
Similar to not having a data infrastructure, some companies get really excited about concepts like AI without having the foundation in place. Machine learning and AI require a company to have a high level of data science maturity, including understanding how to build models, their limitations, and how to deploy them. You might get blamed when their unrealistic expectations meet reality.
Question to ask: how does the company balance spending time on complex approaches like AI with foundational work like cleaning data, checking data quality, and adding logging?
5. No version control.
Mature data science teams use git to keep track of changes to analyses and code. Other teams instead use methods like shared network folders, which don’t let you see when things changed, why they are changed, or previous versions. Occasionally teams don’t share code at all and work just lives on the data scientists individual laptops. Avoid these last groups like the plague. Not having methods of sharing code means the team can’t work together.
Question to ask: how to you share code amongst the team? Is all code shared or just some of it?
6. No clear delineation between people who run reports vs do analyses.
The skillsets required to create and maintain reports, to build data science models, and to put machine learning models into production are all different. If the company doesn’t have a clear way of determining who does what work, you could start your job and end up doing work totally different than what you expected. You don’t want to walk in on your first day expecting to build a time-series forecast and find out your job is to refresh the monthly sales Excel spreadsheet.
Question to ask: how are reporting, analysis, and production-model building tasks split?
Red flags on how they value people
1. A totally non-structured interview process.
A structured interview process means that each candidate gets the same set of questions and can be more equally compared. Not only does [it decrease bias](https://hbr.org/2016/04/how-to-take-the-bias-out-of-interviews), it also requires the team think through what’s important in the people they bring on. If the interview process is unstructured, with the interviewer seemingly asking questions off the top of their head, then it’s a strong sign they haven’t figured out what they want in a candidate and how to get it. If they don’t know what they want you are going to have trouble giving them what they want on the job.
Suggestion: see if they bring a set of questions to the interview, or ask the meta-question of how they chose what to ask you.
2. No time for your questions.
Since interviews are also for you to find out about the company, you need to have time to do so. If there isn’t time for you to ask questions, the interviewer isn’t interested in making you feel comfortable and allowing you to assess your fit.
Suggestion: if you get to the interview and you didn’t have time to ask questions, make a note of it and ask the interviewer when would be a good time to ask them instead.
3. No coding required in the interview.
While programming isn’t the most important skill for a data scientist, it is something you will have to do on the job. The coding part of the interview could be on-site or a take-home test, but it should definitely exist. If the interview process doesn’t include programming it could be for a few reasons: (1) The data science team is new so no one can run the interview. In this case, be aware you won’t have support on the job. (2) The team doesn’t have the time to create a programming interview. This is a sign they don’t value hiring. (3) They don’t program and use BI tools like Tableau and Excel for their work. (4) They trust your resume so much that they don’t need to test you, while is flatting but a sign that they are desperate to hire.
Suggestion: if the interview doesn’t include a programming component, ask them how do they tell which candidates have the technical skills for the job.
4. No plan for your first few months.
Companies put job postings out for good reasons. If they aren’t able to clearly articulate exactly what you’ll be doing in the first few months, that reason is probably “we are totally overwhelmed with work and we are going to throw people at the problem until we can handle it.” That’s an extremely dangerous way to grow a team. What’s worse, this usually happens at the companies that don’t have onboarding processes for new hires. So these situations are extremely stressful for the whole team and that usually falls on you too.
Suggestion: ask if they have a clear project and on-boarding process for your start. If they don’t have an extremely precise answer for this, run.
5. No support continuing education.
Data science is a large and rapidly advancing field and if you don’t keep learning you’ll fall behind. Teams should have some way of helping people keep up, whether it’s by providing funding for online education or attending conferences, monthly meetings where you discuss industry blog posts, encouragement to attend meetups or get involved in open source, or a speaker series. This also shows they’re willing to invest in their people generally.
Suggestion: ask how they support the continued education of the team. Is there funding for conferences or workshops?
6. Inconsistent answers between interviewers about the role
Usually interviews have you talking to many people within the company, including your future manager, teammates, and business stakeholders. If they each tell you different things around the level of responsibility, type of work, what the role delivers, and hours you’ll have to work, they themselves probably don’t agree. If they can’t agree, especially on things related to what work you’ll end up doing, your job will end up being full of conflict.
Suggestion: keep track of what people say in different interviews. If you find an inconsistency, ask why.
While these twelve flags may feel like a lot, companies tend to show none of them or most of them, with only few in the middle. By keeping an eye out of them you can avoid the problem of getting to a job you dislike. Good luck!
