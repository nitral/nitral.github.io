---
title: Tracking the Marathon Run!
header:
  teaser: /assets/images/bfc-marathon.jpg
---
![BITS Firefox Community](/assets/images/bfc-marathon.jpg)
> Introducing Technological Innovation in an Age Old Sport!<!--more-->

My college hosts a myriad of fests throughout the academic year. One of these is an Open Sports Meet that has been around for many years. It's a four-day fest that witnesses an attendance of about 150 colleges with a footfall of thousands of students. The BITS BOSM Marathon is a recently included event of this sports spectacle which has already seen quite an impressive attendance. This year's edition saw me as the event organizer. I, along with others, organized the event on behalf of the **BITS Firefox Community** which is a technical association of my college. The event saw a successful participation of 70+ semi-pro racers among whom was an _**Indian Paralympics veteran**_! Yep, more awesomeness to come...

A marathon is a long race which generally has a track that courses through an urban jungle. A problem that results from this inherent property is the issue of tracking marathon racers to ensure that no one takes the _**'smarter'**_ route to the finish line. Generally in low budget races, checkpoints are made where officials note the bib numbers of racers that pass them. This log is tallied and the final result is compiled. In higher budget races however, [timing chips](https://adventure.howstuffworks.com/outdoor-activities/running/events/marathon-race-timing2.htm) are used to do the same. Mats are placed at strategically placed checkpoints. These interact with the chip, which is usually placed inside the shoe, and synchronize the racer's timings. BITS BOSM is a fest completely managed by **students alone**. We didn't have enough budget to buy these fancy chips. Previous organizers used colored tokens for the same effect. However, I didn't particularly agree with the idea of a marathon racer stopping, submitting his current token and picking up a new one with finesse to go on and run the rest of the 10k. Obviously, this yielded room for some **innovation**.

We wanted something that was Hi-Tech like the Timing Chips but inexpensive like the notepads & bibs used in low budget marathons. So, we set out to build an application that could be used at our checkpoints to easily note the bib ID of each racer that passes them. As this information could be stored on the cloud along with other information, we could process that information in _real-time_ to display statistics. This introduced a spectator element to the race making it more interesting not only for the racers but the passers-by as well. Since we could store a lot of high precision information related to checkpoint timings, we could do **post-race analysis** as well and send the results to racers so that they can improve! Everything sounds good! So, let’s get to it!

As soon as I sat down listing the complete specifications to this project, **a lot of** problems popped up. Most of them had to do with the fluctuating internet connection that was available to us - _Mobile Internet_. Nodes (or checkpoints), fluctuated not only in bandwidth but connectivity as well! Other than this, data was to be aggregated from all checkpoints in a _synchronized manner_. Also, I could not rely a lot on the machines that were available to us in terms of software/environment installed. I started listing all these issues down and the causes and effects of them. Soon, I had a broad picture of the problem that I had to solve. Eventually, I was able to cross these hurdles.

![Marathon Tracking System Architecture](/assets/images/marathon-tracking-system-architecture.png)

I implemented a **client-server architecture** web application that tracked the progress of the racers sampled using strategically placed checkpoints. Data was synced to a **central server** which kept the states of all racers, command logs from clients, and client details among _other_ things. Some of this data was exposed externally through an API that was used to display real-time statistics. On spotting a racer crossing a checkpoint, the bib ID could be entered or the corresponding bib ID button had to be pressed on the **Race Grid View**. This, internally, enqueued a _**command**_ object and issued the same on the **local storage** of the client. In this way, it created 2 copies of the state of each racer leading to _Global storage_ at the central server and _Local storage_ at individual clients. The client implemented a **Postman** object whose role was to deplete the queue _one command at a time_ by sending it to the central server. In the case of a network outage, the commands will keep on queuing for sync up and the state of all racers (with respect to the particular client) would keep being saved on the client. On recognizing a working net connection, work resumes as normal and the client is eventually synced again. The application also featured a **Log Console** which displayed relevant messages that could be used to monitor proper functioning. Additionally, override commands were available that will force a **Sync-Up** or **Sync-Down** (from the perspective of the client). Finally, I also implemented a **Race State Printer** which printed a **JSON** object containing the locally stored copy of all the racers’ states. Other than all this, the application contained some Easter egg commands to override the entire system as well for power users.

I implemented all this using **open web technologies**. This made it possible for me to be sure that the application could run using just a simple browser that is available on every machine. On login, each node awaited a start signal from the central server. The client could however, force itself to start the race in case of a network outage. To tackle the problem arising due to the different start times on different machines, event time was measured as a _**differential**_ of current and the noted start time of the particular machine. The start time was saved on the central server for each checkpoint. If the machine had to be changed, the new machine had to only log in and perform a sync-down once.

It took me about **2 days** to complete everything in the project. Keeping to the Mozilla spirit, I made it open-source for use and further development on [Github - Marathon Tracker](https://github.com/nitral/Marathon-Tracker). The overall experience was enlightening for me as I got to solve problems I had never encountered before. I got to work with some brilliant peers who helped in the development of other aspects of the project such as a **Statistics Display** and **Comprehensive Registration** making it an all round system.

We had searched online and found a similar system used by MIT called [fsTimer](http://web.mit.edu/bletham/www/fstimer/index.htm) for their own Marathon Race. However, it was made only to decide the winners at the final checkpoint. I hope your search for a Marathon Tracking System ends here.

**Thanks for reading!**