---
layout: post
title:  "A little more code review please!"
date:   2021-01-10
categories: jobs
tags: dev remote
description: Most of us working in software engineering can agree that code reviews are important and should be a priority, no matter how busy our calendars get. However, I have seen my fair share of teams that have struggled to carry out their best code review intentions into sustained working process. In this post, I will discuss some code review best practices for both authors and reviewers, so that we can make the process as efficient and enjoyable as possible. A little less conversation, a little more code review please!

---

{% include read_time.html %}

Most of us working in software engineering can agree that code reviews are important and should be a priority, no matter how busy our calendars get. However, I have seen my fair share of teams that have struggled to carry out their best code review intentions into sustained working process.

In this post, I will discuss some code review best practices for both authors and reviewers, so that we can make the process as efficient and enjoyable as possible.

**A little less conversation, a little more code review please!**

<iframe width="560" height="315" src="https://www.youtube.com/embed/WWVMXLSS1cA" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Code review, what is it good for?
[*Code review*](https://en.wikipedia.org/wiki/Code_review), or *peer code review*, is the process through which the **code author** gets their colleagues/peers to check their work. The author typically submits a **pull request(PR)** which contains the implementation of all the changes they are proposing. The author's colleagues typically read and make recommendations on the implemented code changes becoming **code reviewers**.

As mentioned in the intro, everyone seems to agree that code reviews are generally a good idea and they have been thoroughly discussed in [articles](https://leanpub.com/whattolookforinacodereview), [blogs](https://smartbear.com/learn/code-review/why-review-code/) and [books](https://www.amazon.co.uk/Implementing-Effective-Code-Reviews-Maintain-ebook/dp/B08HGLTRPM/). 

A few of the benefits of code reviews are:
- **Better code quality**: code reviewers can find defects or edge cases that the author has not considered before they are deployed to production. The cost of code review time is infinitesimal compared to the potential cost of a production outage 
- **Knowledge sharing**: engineers of all levels benefit from code reviews: newbies learn how to solve complex problems from the more experienced members, while experienced members stay in the loop with all the changes that the team is making and prevent knowledge silos
- **Increase code ownership**: the team gets a sense of solidarity, responsibility and ownership for the whole codebase if the work is collectively agreed on. Code reviews offer the opportunity for everyone in the team to propose solutions and discuss the best solutions to problems
- **Ensure maintainability**: once the team reach a mutually agreed solution, they can also ensure that the codebase is documented and maintainable. The code review process itself ensures that other engineers understand the work of the author, thus ensuring maintainability of the codebase

Code reviews are a very worthwhile time investment and are an essential tool for any team delivering stable production software.  *Unlike war, code reviews are good for many things!* 

<iframe width="560" height="315" src="https://www.youtube.com/embed/01-2pNCZiNk" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Code review, where did we go wrong?
Some ways I have seen the code review process go wrong are: 
- **Dramatically slowing down code delivery**: code changes can be stuck awaiting for review for a long time and slow down delivery. A PR can be stuck waiting for review for days. Finally, the review is rushed and the code is merged to production at the last minute
- **Becoming a box ticking exercise**: code changes are not reviewed in depth and in a constructive way. The code review becomes a box ticking exercise where all reviews are automatically approved, rendering the code review process irrelevant
- **Not sharing with the whole team**: team members approve each others' reviews without sharing them with everyone. Team members are not aware of all the changes made to the codebase

When it goes wrong, the code review process cannot give us the benefits discussed in the previous section. Code reviews then become a cumbersome annoyance that we avoid and ignore. *When code reviews go wrong, it is all our fault!* 

<iframe width="560" height="315" src="https://www.youtube.com/embed/dEw_t9jR8Yc" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## I'm gonna write you a PR! 
Having looked at the benefits of code reviews and how they can go wrong, it's time to discuss what we can do as code authors to make the process smooth and enjoyable for code reviewers. The author starts the whole process and has a vested interest in making sure reviewers can make the best recommendations for their work.

In my experience, code authors should ensure that they:
- **Easily share proposed code changes**: the proposed code changes should be easy to access and read. I mentioned earlier that code changes are typically shared by raising a pull request(PR) and sharing it with reviewers. PRs are hosted on Github, or a similar service, where the reviewers can view and comment. While this is common practice, simply extracting a diff file and sharing it would do the trick as well, as what is most important is giving reviewers a way to easily see the proposed code changes at a time that suits them
- **Write meaningful commit messages**: commit messages are the first thing that code reviewers see when they start the review. They should be well written and offer context to what the changes are aiming to achieve. They should also link to any tickets/issues they are fixing to ensure traceability. [Go's contribution guidelines to good commit messages](https://golang.org/doc/contribute.html#commit_messages) offers some very good recommendations on commit messages 
- **Keep code changes small**: it is tempting to extend scope and implement several things in the same code commit. This is a tendency that is common with less experienced engineers, and a mistake I often made in the beginning of my career. Large code changes are hard to test, stabilize and review. Remember that a code commit does not have to fix a whole ticket. A single code change should be a small, testable piece of work that achieves only one single piece of functionality. It should be complete, but should not attempt to do more than its small scope. This will allow reviewers to focus on the one change and reduce review time, making it easier to fit into their schedule
- **Allow the entire team to see the proposed change**: make sure that code changes are shared with the team and assigned to appropriate code reviewers. Share the code change on the correct channels and ensure everyone in the team is aware of the change. Reviewers with the correct expertise should be assigned for review, going outside the team if necessary

Code changes which are well defined, well tested and provide sufficient context are a joy to review. *As long as your code changes follow these simple rules, you can write me a PR any time!*

<iframe width="560" height="315" src="https://www.youtube.com/embed/qi7Yh16dA0w" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## These PRs are made for reviewing!
Finally, once good code changes are submitted for review, it is time for the code reviewers to shine and do their best work! The code review itself should be an enriching and constructive process for all. 

In my experience, code reviewers should: 

- **Make it a priority to unblock others**: in essence, once the author has submitted their code change for review, the work is blocked until reviewed. It can be difficult to balance delivering own work together with code reviews, but code reviews should be the number 1 priority of the entire team. Unblocking others should always take precedence to delivering own work
- **Offer meaningful constructive comments**: code reviewers should ensure they take the time to write meaningful, well formulated and constructive comments that empowers the author and helps them grow. Comments should take into account the level of experience and knowledge of the author, ensuring that they offer easy to follow guidance
- **Request explicit documentation**: code reviewers should set aside their own experience with the codebase and request explicit documentation which might be useful for a newcomer, even if they themselves don't need it. This ensures that code changes are maintainable and well documented
- **Agree on a team wide review turnaround time**: my recommendation is that the team agree on a code review turnaround time and collectively enforce it, to avoid code changes being blocked awaiting for review. Once this is agreed, team members can block their calendars accordingly to ensure that they have the appropriate bandwidth to deliver these reviews in the agreed time frame

Code reviewers play a crucial role in shipping stable features at speed. Prioritize unblocking others and *review these these PRs that are made for reviewing.*
<iframe width="560" height="315" src="https://www.youtube.com/embed/SbyAZQ45uww" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Parting words
Code reviews are such an important engineering tool: helping us deliver stable software, learn from each other and ensure we are shipping the best solutions. Engineers frequently change hats between code author and code reviewer during daily work, so keeping in mind what to do to make the other person's job easier will ensure that the process remains efficient and enjoyable for everyone involved.

I hope you enjoyed this roundup of code review best practices with a musical twist.

**Happy code reviewing!** 

