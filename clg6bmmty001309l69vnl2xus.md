---
title: "Week 1: MLH Prep Fellowship"
seoTitle: "Week 1 of the MLH Prep Fellowship: Learnings and Insights"
datePublished: Fri Apr 07 2023 09:04:52 GMT+0000 (Coordinated Universal Time)
cuid: clg6bmmty001309l69vnl2xus
slug: week-1-mlh-prep-fellowship
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1680857256819/7b095077-6c6f-4622-9a6c-48a27e3af8e8.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1680858212870/6d825b92-4687-4e64-b78a-824c26e93ea8.jpeg
tags: github, opensource, mlh, github-actions, fellowship

---

## Introduction

The MLH Prep Fellowship is an intensive 3-week experience, during which fellows get to build their portfolio of personal projects, experiment with new technologies, and collaborate in small groups. The Prep Program is designed to quickly build technical skills and experience for candidates who wish to pursue the full MLH Fellowship in the future. Over three weeks, participants work on various projects and learn from experienced mentors.

It's been almost a week in the **MLH Prep Fellowship's April batch** and the journey so far has been nothing short of amazing. In this blog post, I will share my experiences of the first week of the fellowship, including key insights and learnings.

## Week 1

Our pod leader, **Aksh Gupta**, started the program off with a unique introduction session where we were asked to introduce someone else in our pod rather than ourselves. This was a great icebreaker and helped us get to know each other better right from the start. Aksh then briefed us on the plan for the next three weeks, the projects we'll be working on, and how to get help when stuck. He also introduced us to **stand-ups** and **Lightning Talks**.

**Stand-ups** are daily check-ins where we discuss **what we accomplished** since the last stand-up, **what we want to accomplish** before the next, and **any blockers** we faced in between.

**⚡ Lightning Talks ⚡** are opportunities to give **presentations** on any technical topic of our choice. I'm excited to give my **Lightning Talk** next Thursday and share something useful with the rest of the pod. Yesterday, **Tomas** gave a wonderful talk on the **Jekyll** framework.

Over the next few days, I had the opportunity to connect with my pod mates from various parts of the world, including **Spain, Nigeria, Canada, the USA,** and **India**. It was truly awesome to learn about their cultures and histories and about their hobbies other than coding too. And I am sure this experience of **collaborating** with people from different **time zones** will come in handy when working in **remote + global teams**. 🕰

This week, we worked on a portfolio website project, which had **14 open issues** to work on, and we were each paired with a partner and assigned two issues to work on. I started working on an issue to write a **GitHub Action** to **compress images** being pushed to the repo, which helped in **improving website loading time**. I worked with my pod mate, **Ikeh**, on this issue, and we submitted a PR that was merged. It was nice to learn about **GitHub actions** and **workflows** since I had seen them a lot in open-source projects but never understood what goes on under the hood.

I also worked on another issue with **Priyanka** that had three tasks, for which I submitted two separate **PRs**. However, **Aksh** made me realize that creating multiple PRs for small issues is not a good practice. I also learned about keywords like **Fixes, Closes, and Resolves,** and how they work. This was a valuable lesson, and I'll keep it in mind for future projects. Quoting him below:

> " When you write "Fixes #13" in a PR description, it automatically closes that issue when the PR is merged. So if two PRs have "Fixes #13" written on them, the issue would get closed when the first PR is merged, when the issue is actually not completely solved."

By the end of the week, I also received feedback from **Aksh** regarding **duplicate commits** on two of my PRs, which I later understood was due to a **branching** mistake that I had been making for a long time. The pod members helped me find solutions, and everyone was always willing to help out whenever needed.

**So here's what went wrong:** I was working on two branches and making changes respectively. But when I tried to merge either of those branches, commits from the other branch also started attaching themselves to that PR which was not intended.

**Explanation:** If you created `branch3` while on `branch2` using `git checkout -b branch3` and made additional commits on `branch3`, then when you merge `branch3` into `master`, the changes from both `branch2` and `branch3` will be included in the merge commit that is created.

**Solution:** As pointed out by my pod mate, **Lakshya**, switching to the main/master/default branch before creating a new branch helps. I made a dummy repo on GitHub and tried it out for myself and it worked! 🥳🥳🥳

## Conclusion

Overall, the first week of the MLH Prep Fellowship was a great learning experience for me. The program helped me to improve my technical skills and knowledge while providing me with the opportunity to connect with people from diverse backgrounds. I'm excited about the upcoming weeks of the fellowship, where we'll be working on a ReactJS project.