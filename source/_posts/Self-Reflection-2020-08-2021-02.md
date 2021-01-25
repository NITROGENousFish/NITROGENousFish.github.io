---
title: Self Reflection / 2020.08~2021.02
date: 2021-01-25 20:06:11
tags:
- 反思
---

>In the end of a semester, a student will be requested to send a self reflection email with a sample as below:
>
>For the self reflection, I hope you can answer the following questions:
>
>- Did you work hard enough over the past period (i.e., past semester)? If so, illustrate the supporting evidence. If not, explain why you would have such situation and explain how you would be able to improve the situation in the next period (i.e., next semester) (with concrete action plans or things)?
>
>- On what aspects did you work in a **desirable way** over the the past period? Illustrate the details.
>- On what aspects did you work in an **undesirable way** over the past period? Illustrate the details. Also explain why you would have such situation and explain how you would be able to improve the situation in the upcoming period (with concrete action plans or things)?
>- Any other comments you would like to self reflect on your research activities, styles/processes, skills, and/or accomplishments or anything else in the past period, especially with respect to work hard, work smart, and work wise?



<font size=6>This</font> semester, I think I have worked hard enough, but there is still space for improvement. From the time I chose the research topic of Taobao intelligent stress testing, I knew that it would be a highly engineered project that required a clear schedule for coding. In this semester, I was able to push the project every week according to the plan set from last week group meeting. This is an important prerequisite for ensuring the completion of the project in the three months. I think I have pushed myself enough. However, in the process of this project, due to my poor definition and understanding of the problem, I actually waste a lot of time, including trying inappropriate models (such as LightGBM, GRU and later I found this problem is actually multivariate nonlinear regression, which means all of these models do not work), and trying some approaches that are not actually related to the project (such as fuzz testing). Although these works can be regarded as a valuable experiment from my perspective, but it can also be regarded as "useless work", and I think it is a flag of "not working hard enough". I did not think sufficiently in the early stages, which led to this "useless work".

<font size=6>I</font> think I worked in a desirable way with the following two aspects: First for my internship in Alibaba, the weekly CI/CD process is my best working status. Second, since joining the ASE group, my research topic gradually focus on software engineering. Besides my internship, I began to read papers independently, especially those that were not completely related to my work, such as quantum program testing, and some papers on GUI Testing (recently I'm skimming all papers in ICSE in the past five years, and taking some notes based on my understanding). I confirm that it is very meaningful to read as many excellent papers as possible after listening to Prof. Xusheng Xiao's sharing this week.

<font size=6>I</font> think there are two aspects that I'm not satisfied withmyself. **The first is that the time utilization is relatively low at work**. I tried deep learning models and wrote preprocessing code for about a month, and took three weeks parallelizing my training process , and did engineering implementation for a month. There are three factors that lead to low efficiency: complex engineering structure, slow implementation speed, and unfamiliarity with theories. The first part is actually inevitable. For scalability and well-designed structure, I've sacrificed some time for encapsulating and testing. For the second and third parts, I think there is definitely room for improvement. Basically, I spend half of my time learning theories and code writings (such as selecting models, learning to write async python, web frameworks etc.), a quarter of my time testing code demos to understand knowledge and confirm code result, and the last quarter is the actual engineering part. **The second is that the free time is not fully utilized**, because a lot of time is spent at work and no extra time is reserved for me to learn other things. Specifically, I only read a quarter of the basic tutorials of Rust, and I did not read more than 10 papers outside of my work.

<font size=6>Because</font> there is no course this semester, I can concentrate on some things I want to do. In my spare time, I started to read the source code of third-party libraries in Python (Tensorflow, Subprocess, Asyncio, Shutil etc.), so I can deeply understand their inner logic and master how to use it. My mathematical basis is not good, so I am also looking at some mathematical derivation related to data science (especially machine learning and linear regression analysis), although some parts are still difficult for me, but there is nothing bad about understanding the principles. In addition, under the recommendation of developers in BUPT, I started to learn the Rust programming language. Compared with other languages, the learning curve of this language is quite steep. I have not yet written projects in Rust, but at this stage I think the design and concept of Rust is more suitable for software engineering research than other languages such as C. Also I build my first "In Use" homepage on github (I had some personal homepage before, but they are no longer maintained just because I'm too lazy), hoping to note down some of the knowledge that I've learned, and gain experience.

<font size=6>In</font> order to quickly index, I just put solid action plans here.

- **For work time utilization in UNDESIRABLE part**
  - Note down most of the knowledge I studied into web docs, avoid forgetting and reduce time for searching and learning again. The specific action is to ensure that my homepage is updated at a regular rate. Although it is a stupid method, it is still extremely effective for me. (Work wise? I think.)
  - In terms of speed, I need more practice, and a certain amount of code must be guaranteed. The actual code of project in Alibaba intern has about 15 files, with above 2,000 LOC. The playground code for functional testing has about 5 files, with above 1,000 LOC. At least the LOC number for the next semester cannot be lower than this semester.
- **For free time allocation in UNDESIRABLE part**:
  - My current plan is to use morning time to read papers and learn new knowledge (such as Rust :D) , just before working time is fine. So that my free learning time will not be squeezed by working time.
  - The time allocation at weekends has also improved to a certain extent. I now reserve Saturday as my leisure time, and regard Sunday as a weekday. I will try this schedule for a few months to see if there is any actual effect.
- **For work hard&wise part:**
  - For the scenario of determining solution, it is necessary to conduct as small experiments as possible, not engineering-level experiments, otherwise it will take a lot of time to do engineering stuff. A simple experiment is needed before the large one.
  - For scenario where "multiple solutions have been determined and all need to be experimented": as above, improve my implementation speed.
  - In addition, I must fully understand the problem before I solve it. I personally find that it's helpful just describe to my goals and solutions with others and will find some flaws in my problem definition. Let ideas go through a "peer review".



<font size=6>**ABOVE ALL: WRITE MORE, SYNC MORE, DON'T BE LAZY.**</font>

Honored to join the ASE group and keep improving myself !