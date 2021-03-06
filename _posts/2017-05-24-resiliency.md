---
layout: default
published: true
---

At [PagerDuty], we have a monthly "Hack Day," where everyone can spend a day working on something of their own choice. Obviously, lots of the "hacks" are features, but some are internal tools, and some are really great ways to make the company a better place that have nothing to do with software. Ask any Dutonian about the post-it notes, for example.[^post-it]

[PagerDuty]: https://www.pagerduty.com/careers/
[^post-it]: No, I won't try to explain in "print." You need to hear it from a human being to absorb the full emotional impact :-)

---

[![Photo by Kevin Megens / Artibite](/assets/images/balloons.jpg)](https://www.flickr.com/photos/artibite/16757272310)

After the hacks are done, we have a little "demo" ceremony, everyone gets five minutes in front of the company to show what they've done. Yes, it's "Show & Tell" for adults. And like a really good classroom full of nice kids, the audience is incredibly supportive, especially to first-timers and new Dutonians. I really don't have enough words to describe how positive the culture feels to me.

After the demos are done, the company huddles a bit and we ask ourselves if any of the hacks deserve to either go straight into production, or go into our product pipeline to get some further tweaking and become full-fledged product features.[^me]

[^me]: At least one of my "hacks" is in production, but if you don't work in PagerDuty engineering (and likely even if you do) you have no idea what it is. It's something that "just works" and nobody will notice it unless we remove it.

So that's our "hack day" structure, and last Friday we worked on our hacks. We usually demo them on the following Monday, but that was a Canadian holiday, so we demo'd them yesterday. My colleagues on my team demo'd four very fine new features, and honestly I can't wait until they are in production. It's great stuff!

---

[![Photo by Kevin Megens / Artibite](/assets/images/windy.jpg)](https://www.flickr.com/photos/artibite/16943739191)

### demonstrating my hack

I pride myself on "Giving Good Demo." I have experience in sales and public speaking and since I'm demonstrating something I care about in a personal way, the enthusiasm flows freely. But I can't help feeling a little pressure. With every good demo, I feel like the expectation that the next one will be as good, or even better, mounts. And I was following such great work! How could I compare?

I walked to the podium. And... The screen did not work properly, I got some horrible VGA-resolution situation. I made light of things with a joke while a colleague opened up screen preferences and got it sorted. Not a great start, but onwards.

I then demonstrated a thing where for big requests, the web app would break a request up into chunks, and send them all asynchronously to the API. This parallelism has some tradeoffs, and the team and I want to explore whether this is, overall, a big win. The UX problem to solve was that up to that moment, all requests either succeeded or failed. Now I was introducing the possibility that if a single action resulted in multiple "chunks," some might succeed and some might fail.

So for my demo, I triggered an action that would produce two chunks. And behind the scenes, I introduced some randomness such that each chunk had a 50% chance of failing. In reality, well-formed requests fail infrequently, but it can happen, especially when internet connectivity is suspect. So it's an important feature.

But the important failure was my failure to do basic math. If there are two chunks with a 50% chance of either failing, and I want to demonstrate how the UX handles partial success, there is only a 50% chance that my demo will work the first time. And no, it did _not_ work the first time.

I had to try it again, and it took enormous coördination to move the mouse with one hand, while standing on one foot and kicking myself with the other. Luckily, I was not trying to chew gum at the time. I did get it work as planned, but now I had two hiccups. Oh well, on to the big finale.

---

[![Photo by Kevin Megens / Artibite](/assets/images/books.jpg)](https://www.flickr.com/photos/artibite/16918772026)

### the finalé

I had another feature that involved being able to cancel certain types of long-running operations. You know, like when we accidentally start copying the contents of your entire hard drive to a folder on our network server.

There was some code that put up a dialog box with a cancel button, and as befits the "hack" status, the cancel button closed the dialog, and I had some code that canceled the operation when it detected that the dialogue was dismissed. This was a mistake! It worked like a charm in my testing, but what I didn't know was that there was all sorts of JavaScript asynchronicity involved in the route from mouse click to button press to dialogue close to the code observing the dialogue box's status, to cancelling the concurrent task.

I look forward to discussing the exact race condition mechanism at the next [EmberTO Meetup], but for now, the key thing is that the feature only worked if I clicked the cancel button in the first four seconds of a five-second operation. After that, it carries on and doesn't notice that the dialogue was dismissed until it is done.

[EmberTO Meetup]: https://www.meetup.com/Toronto-Ember-JS-Meetup/

I never noticed this before, because when testing, I tested. I didn't stand there delivering patter about the importance of being able to cancel operations. I just canceled them. So when I clicked the button, my feature...

Just. Didn't. Work. In front of the entire company!

---

[![Photo by Kevin Megens / Artibite](/assets/images/nudge.jpg)](https://www.flickr.com/photos/artibite/16943393862)

### the lesson learned

Now, there is a lesson to be learned about AV projectors, and settings, and about doing dry runs before the event. There's another lesson about basic mathematics, and probability. And yes, there's a lesson about race conditions.[^race]

[^race]: Actually, race conditions are hard, frameworks are finicky, and sometimes the best way to find a bug is to demo the software in front of a crowd. So maybe there's no lesson about race conditions.

But lessons or not, the "demo gods" are capricious. One day they smile upon us, and another they cast us down. Being better just means having them smile more often. But we can never be perfect.

It's emotionally draining to give a demo, even a successful one. It hurts to have a demo go wrong in front of people we care about. I hurt, you hurt. When we feel trepidation before going on stage, we are not alone. We all feel it, even those of us with "experience" and a "blog" and invitations to fly around the world to stand on stage in front of people. *We are not alone.*

So instead of trying to be perfect, instead of never taking chances, or trying to avoid ever failing under the spotlight, we should seek to be **resilient**, and weather the storms, and rebuild when our code breaks.

This lesson I learned, and keep learning, is this: A strong, supportive culture—like we have at [PagerDuty]—helps us bounce back. It doesn't mean we won't screw up in a demo, or check in some code that causes a SEV-1 one day, but it does mean that people will support us and work with us and drag us back from our embarrassment and support our "feels."

That culture, that support for us when we are vulnerable, is the foundation of our resiliency. And as noted above...

> Resiliency is more important than perfection.

---

### notes
