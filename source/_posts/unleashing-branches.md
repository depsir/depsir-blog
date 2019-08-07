---
title: Unleashing Branches
date: 2019-08-07 17:08:53
tags:
---

Feature branches got an incredible boost by the advent of git and gitflow. They become so popular that we almost feel compelled to use them.

Don't.


What are feature branches?
--------------------------

From a technical perspective, they are just plain normal branches. The difference resides in the feature word that attaches semantic meaning to those branches: a feature branch is a branch that contains a feature.

We could discuss for hours on the meaning of "feature" in the software development but let's settle onto this simple definition: "A functionality of our software". It could be something like "Create a new blogpost" , "Add an image to a post draft", "Delete a blogpost".

The definition is not really important in this context but we should agree on this simple fact: whatever the definition of feature is, the smaller the feature is, the better:

- From a business point of view, a small feature is implemented quicker and therefore reaches the market in less time, generating revenues earlier.
- For the project, a small feature means earlier feedback from the customer, enabling reactivity to drive towards success.
- For the developer, a small feature means fewer conflicts and less headaches with merges.

When we are taught to use feature branches, we, starting by me, understand that there is a one-to-one correspondence between a feature and a feature branch, and from that, we end up with some rules wired in our brain to the point that we feel uncomfortable in acting differently:

- start and commit a feature on the main branch (develop, master, trunk, wherever you merge feature branches to)
- merging the feature branch before the feature is completed
- committing a small feature on the main branch without any branch
- using the same branch for two features
- ... anything that goes against the feature branch dogma


The reason underneath feature branches
--------------------------------------

What are the advantages of feature branches? One could say that they enable code reviews through merge requests, but this is not relative to feature branches, it can be done in a non-feature branch too.

They are useful because I can generate automatically the changelog for the new release, but I doubt that this is the reason why we decided to adopt feature branches.

If the customer decides, during the development of a feature, that the feature is not needed any more, if it's contained entirely in a branch it's easier to drop it. Now look me in the eyes and tell me how many times this happened to you. Am I the only one to think that it is insane to adopt a technique that makes easy an operation that we perform a handful of times in our life and then have slow and manual activities we perform daily? Tests, code style, build, deploy, ... an endless list. Without counting that removing an uncompleted small feature should be easy enough to be done manually, reverting some commits.

But.. If I don't use a feature branch, at some point an incomplete feature will be present on the main branch and eventually will be deployed to production before it's done! No customer would want that! And this is one good reason to use feature branches, even if it is not the only solution.

Given our definition, an incomplete feature is something that do not work so it must be hidden and not reachable by the user. There are plenty of ways to deliver an incomplete feature without upsetting the customer:

- feature toggles, i.e. a flag to enable or disable entirely a feature. Of course we would commit the incomplete feature behind a turned off feature toggle
- not reachable code: instead of starting from the ui, we could start from the business logic which, not being used, could be deployed to production without trouble.
- start adding a new api: in many cases a new endpoint can be added without causing problems. This is a variation of the unreachable code trick.
- new version of an api: another variation is to create a new version of an existing api. This requires our apis to be versioned.
- hide the ui that triggers the new feature: without configuring a feature toggle, we could simply set display: none to the button that deletes the blogpost

Not everyone of the tricks above is applicable every time, and developing software in this fashion could be risky without experience, but the key idea is that feature branches are one of many ways of solving problems with their pros and cons.

Cons? Of course there are cons.. and they are the exact same cons for normal branches, with the aggravating circumstance that feature branches will last longer: they delay integration, which leads to late feedback, which leads to slower responsiveness, and also harder merges. In the best scenario a feature will be small enough to be completed in one or two days, which means one or two days to be waited for feedback and merge.

When to use a branch?
---------------------

At this point I hope that the one-to-one correspondence is starting to look strange and restrictive.

Imagine that I'm implementing a feature and while doing so, I perform a refactoring that is useful also for other features. One simple option is to merge the branch into develop so that the other feature could integrate the refactoring. But I'm merging a feature branch before the feature is completed! Ok now just forget the feature thing. I developed a refactoring on a branch that has been now merged. Is it so disturbing? But that branch also contained part of the feature! If that part of the feature do not expose to the user a broken behavior it should be ok, isn't it?

Forgetting the feature connotation put everything in a different light.

I have to implement a small feature, a couple of lines at most, just one commit, should I use a feature branch? Why should you? What benefits do you obtain?

I'm implementing a complex feature in a feature branch. I feel that its becoming too much code and I would like to review and merge this part, which is tested, working and do not expose a broken behavior to the user. Can I merge that branch? Why not? You will get feedback earlier and other developers could start to integrate their feature with your changes, possibly avoiding conflicts at all.

At this point the next logical question is: when should I use a branch? Should I use branches at all?

There are few situations that require a branch. I can think of these:

- it's 6 pm, I'm in the middle of a feature but the build is broken or the tests do not pass yet. Instead of leaving these changes on my computer only, I could push them to a remote branch so that they are safe on the server without breaking everyone's build
- I'm doing  remote pair, we want to switch role but our tool is not good enough to allow remote control. Push to a branch allowing your pair to download it and keep working. Of course it is needed only if you cannot merge on develop, e.g. broken build, broken functionality exposed, ...
- I have to switch context and I want to save on the server my incomplete broken exposed feature. Branch to the rescue.
- I want a review on my feature, which is scattered among many commits. In this case it could be easier to do the review from a merge/pull request

In many other cases a branch is one of the tools you can choose to solve a problem.

Wrapping up
-----------

Branches are a powerful tool at our disposal. Constraining their possibilities with the feature connotation  just because we are told to is nonsense. Always think and choose the right tool for the job!

Acknowledgments
---------------

I would like to thank the [Milano eXtreme Programming User Group](https://www.meetup.com/it-IT/Milano-eXtreme-Programming-User-Group/) that discussed with me about trunk based development, stimulating my mind about what I wrote in this article.

I would also to thank many colleagues that listened to me while I was annoying them with my theories about branches
