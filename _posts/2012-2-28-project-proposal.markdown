---
layout: post
---

As an organizer, I want to be able to quickly gauge a crowd's opinion in real time. For my term project, I will build an app that can be used on any phone or browser to create, vote, and monitor these threads. Organizers will have full control, such as limiting time period or votes needed and adding features like group chat, depending on their needs.

The focus of this app will be to make it so easy to set up a real-time poll that I can use it anywhere I am. I will create the entire platform and related services:

* Backend API for threads and users
* Web client
* Apps for phones and tablets
* SMS integration
* Email integration
* Facebook integration
* Google Calendar integration

From an interface perspective, in order to be seamless, the app needs to work with the tools that people already use. That's why SMS is the baseline for creating, voting, and even monitoring&mdash;every phone supports it. From there each component is a progressive addition, with the interface changing to match the type of device.

While this is challenging due to the number of independent parts, I'm confident that I can build this entire system. I have build apps on top of all of these platforms before, now I just need to integrate them all into one system, which I have already modeled through my API.

This is how the system would be organized:

![Architecture Diagram](/images/architecture.png)

One of the key challenges that I anticipate is the ability to go through the entire system with no login, just a phone number. I think minimal set-up is key to a seamless user experience, and will design the entire app around this base case. If users were only using mobile phones, this would be easy. But because users may also log in over the web, I must have a way to link a person's email and phone number on the database level. For security, I also need to make sure that people can only participate in threads they are supposed to, which requires cryptography.

I will use several methods to gauge the success of this app. First, I will use a survey to gauge broadly how people are currently gathering opinions. Once I have a prototype, I will conduct usability studies with a small group of users to pinpoint problems in the interface. I will also gather metrics on usage and participation from within the web and mobile client. Using A/B testing would also allow me to judge elements that people respond to best.

I'm excited about the possibilities that building this system offer. If all goes well, I know that this will solve a real problem for me, and be awesome doing so.