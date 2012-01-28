---
layout: post
title: A Proposal for a Distributed Development System
---

I started working at [Hoot.Me](http://hoot.me) over the summer. It's currently a four-person team. Our CTO spent the summer working with us remotely, and now that I'm in school, I'm doing the same thing. So our workflow has evolved to accomodate those needs.

We use a combination of communication tools that are convenient: Github, Trello, Google Talk, and our phones. The most noticeable thing about these tools is that they are either asynchronous or synchronous, but not both.

We also use several internal tools to manage our code, files, and infrastructure: git, Dropbox, Fabric and Chef. I'll go into the role each plays later, but like our communication tools, they each serve one purpose for us.

This workflow works well with such a small team, but a critical question we have to ask ourselves is, "Does it scale?" As a team grows, the level of communication overhead it needs will increase, a core tenet of Fred Brooks's book on the practice of software development, [The Mythical Man-Month](http://en.wikipedia.org/wiki/The_Mythical_Man-Month).

A well-designed software development system cannot eliminate the additional communication overhead of a larger team, but it can minimize it.


### Development needs

Every component of the Hoot workflow addresses a need we have. These needs can be broadly categorized as:

* Data storage
* Automation
* Management

The ideal software development environment for us will address all these needs in a centralized way, but be flexible if the underlying tools change.

#### Data storage

Code, graphics, plans, and other files need to be shared freely among team members. But there is not one solution that works for any type of file. The type of data dictates the actions that can be performed on it.

Dropbox is a good solution for static files because it every member has an identical state. For example, a logo file does not change often, but if it does everyone instantly gets the update. However, that model is impractical for code. If I'm working on a feature for an upcoming release, it needs to be separated from the stable version of the application until it's ready for production. A system to store code needs to handle states better than Dropbox.

A version control system's core function is to track changes to files.<sup id="ref1_1-27-12"><a href="#footnote1_1-27-12">1</a></sup> But a good version control system implements branching and merging, transforming a linear history into a tree. Branching enables parallel development of features, much like [parallel computing](http://en.wikipedia.org/wiki/Parallel_computing) makes code run more efficiently.

While git is a general-purpose version control system, one strategy we use at Hoot keeps us from making mistakes: **the `master` branch is always ready to be deployed**. Any major changes to the application are done through feature branches, which have descriptive names like `upgrade-libraries`. Enforcing a rule like this actually makes the system more usable, and asynchronously communicates the current software status when developers check the remote server.



#### Automation

Why do something manually if you can do it with code? At Hoot, we don't use automation as extensively as we could, but using any sort of automation increases scalability. It allows a team to deal more abstractly with tasks like deploying files, starting servers, and testing.

[Fabric](http://docs.fabfile.org/) is a Python library that lets you build custom scripts that automate the building and deployment processes. For example, we use the command `fab production deploy` to automatically ssh into the `production` machine, pull the latest revision on the `master` branch, and restart the server.

[Chef](http://www.opscode.com/chef/) is a Ruby framework for systems management. It handles machine configuration, making it incredibly easy to launch new servers.

While we don't use it, git enables a number of [command-line hooks](http://book.git-scm.com/5_git_hooks.html) that can automate tasks like deployment in a similar way as Fabric. A useful hook might be to automatically run unit tests before every commit, and then deploy to production if it's on the the `master` branch.

Automation can encompass any internal tool that increases reliability and speed, which is the purpose of building software in general. It's critical to a software development team's scalability to have the right tools, and the ideal collaboration system is actually a programmable tool itself.

#### Management

A good development process will create meta-information, which is used iteratively to control, analyze, and improve the development process.

A small team working in the same location may not ever need to write down the meta-information, but having a remote team forces you to communicate better. One of the realizations I've had from working remotely is that synchronous communication tends to be undocumented, while asynchronous communication is by definition available after it's created.

Both communication modes are critical at Hoot, though in practice we are more likely to call each other or use Google Talk to chat rather than Trello or Github. The benefit of synchronous communication is in decision-making, because it is easier to come to a conclusion after a quick phone call than over a long email thread. In contrast, posting a bug on Github or a card on Trello is information that needs to be accessed later, or that takes time to respond to.

Github in particular has been a great example of a project management tool that makes meta-information easy to create and access. It's another layer above git that adds bug tracking, code review, and visualizations, which are essential for large software projects. However, it lacks the functionality and connectedness that would make it ideal for integrating everything.


### The proposal

So how can we build the perfect collaborative system for Hoot? It turns out that if we design it as a series of plugins to a core system, then it can be extended to fit the needs of any development team.

The core environment needs to support both synchronous and asynchronous communication. The only existing tool that can achieve that is a text chat, because any text can be saved for later.

To extend this chat environment, you could write any script that takes text input (one per line) and returns text output (or not). To extend the system entirely, like to add a feature, a more complete module would have to be built that hooks into a strong event-driven 

Group chat would likely be the default, but having one-on-one chats with colleagues can help to focus in on particular problems in exactly the same way a phone call would work now. Working with text allows search, but additionally links, files, and other media could be embedded directly in the chat.

The key extension that has become popular recently is to use a chat bot like [Jenkins](http://jenkins-ci.org/) or [hubot](http://hubot.github.com/). Imagine that instead of running `fab production deploy` from a command prompt, you run a similar command in the chatroom. Suddenly it becomes trivial to see what the last update was to the production server; it's just a search away. Even if a team wanted to use completely different methods than we do at Hoot, they could organize their internal tools using extensions and a chat bot.

#### Data storage

We could continue using the same differentiated data model, and even the same services, on the ideal collaborative platform. git, svn, and other version control systems can be integrated through their command line tools. Dropbox has a web API. Anybody could write an extension that would add support for such data, and allow it to be referenced anywhere in the chat. For example, git already uses unique hash codes to represent data, and that could be used in chat to reference files.

#### Automation

A chat bot can be as simple as a command line program that listens for text and can respond with other text. But even though the input and output are simple, it can handle complex tasks in an automated way. I would imagine that if I could use this system, it would be easier to deploy code to servers from chat than a terminal, because the scripts that the chat bot would run automate that away.

#### Management

Would you like bug tracking? There's an extension for that. Pull requests and code review? That can also be extended. Once you allow scripting, you can make your system add as much meta-information as you want. I think the minimum useful set of management tools is a to-do list and code review, but other people may have differing opinions.


### Why doesn't this exist?

What I've just described is an incredibly general system that can be extended to become exactly what a team needs. But in my research for this piece I spent some time comparing project management tools, and have come to the conclusion that [Beanstalk](http://beanstalkapp.com/) would be the best tool to use at Hoot. It already incorporates a number of the ideas I've talked about, with built-in data storage, automation, and management functions and the ability to use popular third-party alternatives or write your own. It combines the flexibility of Github with Basecamp's great project management tools, which is one of the goals I had in mind when designing this system.

But even more than our desire for a better project management system, the system I've described sounds like something I want to build. After all, it's just a chat. But the interesting challenge for me is to build a system that other people will want to contribute to. In fact, some of the concepts I've mentioned here are already being implemented in the Hoot Facebook application.

---

<ol class="footnotes">
<li id="footnote1_1-27-12">By that definition, Dropbox is also version control system because they store a <a href="http://www.dropbox.com/help/11">complete history of your files</a>. But without branching it's not a good tool for coding. <a href="#ref1_1-27-12">&#8617;</a></li>
</ol>