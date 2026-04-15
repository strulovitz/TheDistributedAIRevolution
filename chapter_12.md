# Chapter 12 — How the Hive Hears and Sees: One Trick in Two Dimensions

---

This is the chapter I did not plan to write.

When I sat down with my Claude tonight, the only thing on the agenda was a small piece of housekeeping for the KillerBee project — the project that lets me run a hierarchical hive of cheap virtual machines on my own desk so I can watch how the splitting and the combining actually behave when I push it past two or three layers. We were trying to figure out how to test the *vision* round of that hive — how to give the swarm a real photograph and ask it a real question and watch the answer come back from the bottom of the tree to the top.

I asked Claude to walk me through what such a test would look like. Not which model would run where. Not which port would talk to which other port. Just: *what photo, what question, what happens.* In words. So I could see in my head whether we were testing something real or testing something we were fooling ourselves about.

What happened in the next forty minutes was the discovery of a principle that — if I am right — is as big as the original idea this whole book is about. It is the answer to a question I had been quietly avoiding for two years: how does a swarm of small, cheap, distributed AIs perceive the world? Not pretend to perceive it. Not hand-wave a description of perceiving it. *Perceive it.* The way an animal with eyes and ears does.

I did not know I was looking for this answer. We found it by accident, in the gap between a question I asked and an answer that turned out to be wrong.

The principle that came out of that conversation scales across **four dimensions** — sound is one dimension, an image is two, a 3D map of the world is three, and a 4D model of the world over time is four. In this chapter I am going to walk you through the first two: sound and sight. The next chapter will scale the same principle to all four, and that is where the discovery becomes really enormous, because the four-dimensional version is the answer to a question I have never been able to answer honestly until tonight: how a *swarm* of distributed perceivers can build up a complete model of a physical place and the things moving through it, without any single member of the swarm ever seeing the whole picture.

But first, sound and sight. The trick that makes everything else possible.

I am going to walk you through it the way I actually walked through it, with my Claude, in real time. Because the polished form would hide the *why*. And the *why* is the only part that matters.

---

## The walk-through I asked for

I said: imagine the simplest possible test. A single photograph, a single human user, a single question. Tell me what the hive does, stage by stage.

Claude gave me a beautiful, clean, plausible walk-through. It went like this.

The photograph is an outdoor café in the late afternoon. There is a waiter in a white shirt carrying a tray of drinks. Four people are sitting at a wooden table eating — two adults, two children. A small brown dog is curled up under their table. To the left, a bicycle leans against a lamppost. A chalkboard near the entrance lists today's menu starting with "Espresso €2.50." The awning over the entrance has the name "Café Roma" in faded paint. Across the street a small delivery truck is parked. There is a round clock mounted above the entrance showing 4:15. The sun is coming in low from the right side of the frame, casting long shadows from the people across the cobblestone street. The trees in the background are fully leafed and green.

The user asks the hive: *"Describe everything happening in this photo, and tell me what time of day it is, what season it is, and what kind of business this is, with evidence for each conclusion."*

The hive does the following, Claude said.

**Stage one** — the user hands the photo and the question to the top of the hive. The Queen at the top.

**Stage two** — the Queen does not look at the photo yet. She looks only at the *text* of the question. She uses her language model to think: how do I break this question into pieces that can be answered in parallel? Her language model produces seven sub-questions:

1. What people are visible and what are they doing?
2. What animals or pets are visible?
3. What objects are visible and where are they positioned?
4. What text or signs are visible, and what does each one say?
5. What does the lighting tell you about the time of day and the direction of the sun?
6. What seasonal clues are visible?
7. What kind of business or location is this, and what evidence supports that?

Each sub-question is independent of the others. None of them needs the answer to another to be answerable. That is the splitter's job.

**Stage three** — the Queen sends the seven sub-questions and the photo down to her sub-coordinators, who in turn send pieces of the work down to the workers underneath them. Each worker gets one sub-question and the same photo.

**Stage four** — each worker runs its little vision model on the photo and produces a short text answer about its assigned sub-question. The "what people" worker says *"I see a waiter in a white shirt carrying a tray, four people seated at a table — two adults, two children, a girl reaching for something."* The "what objects" worker says *"I see a wooden table, plates, a bicycle leaning against a lamppost, a chalkboard near the entrance, a delivery truck across the street."* And so on.

**Stage five** — the workers send their text answers back up the tree. The sub-coordinators bundle their workers' answers into paragraphs and send those paragraphs up to the Queen. The Queen merges all the bundles into one final answer, in plain language, and gives it back to the user.

It looked correct. It looked elegant. It is the kind of design that sounds right when you say it out loud. I was about to say *yes, do that, build the test* and move on.

I stopped because something felt wrong, and I could not yet name what.

---

## The two ways we were fooling ourselves

I sat with the discomfort for about a minute, and then I saw it. There were two things wrong, not one. They were both invisible if you only looked at the design once. They were both obvious once you noticed.

**The first thing.** The user's question, in Claude's example, was *"describe people, time of day, season, business type, with evidence."* That is not a question a human being asks. That is a teaching exercise. A real human user, holding a photo, asks one of two questions: *"what is in this photo?"* or *"what is happening in this photo?"* Flat. Open-ended. No structure. They do not pre-decompose the question for you, because they do not yet know what is in the photo — they are *asking the photo*. They are blind, and they want their assistant to look.

But the design Claude gave me had the Queen at the top trying to split the user's question *before looking at the photo*. With the artificial structured question, the splitter had something to split — *people, lighting, season, business* are pre-named categories. With a real question, the splitter has nothing to split. *"What is in this photo?"* cannot be decomposed by a text model that has not seen the photo, because there is nothing in the words *"what is in this photo"* to suggest what categories to ask about. The best a text-only splitter can produce is a generic checklist — *"are there people, are there objects, is there text, are there animals, what about lighting"* — which is not real decomposition at all. It is *"look at the whole photo and report on every category in parallel."* It is the same as having one model look at the photo once. There is no parallelism in it. There is only the appearance of parallelism, which is worse than no parallelism, because you also pay the overhead of pretending.

**The second thing.** Even setting aside the fake question, the workers in Claude's design were not actually doing parallel work. They were each looking at the *whole photo* and producing a *narrow answer*. A vision model does not have a *"only look at this part"* mode. It loads the entire image as a tensor, runs the entire forward pass over every pixel, and *then* its output is conditioned on whatever the prompt asks about. The cost of asking *"what is the dog doing?"* is exactly the same as the cost of asking *"describe everything you see."* Restricting the question does not restrict the work. So when four workers each ask a narrow question of the same image, they are doing the work of four whole-image analyses for the price of four whole-image analyses, and then artificially trimming their answers. The "parallelism" is fake. The workers are doing the same job four times.

In *text*, splitting works because each worker only sees its chunk of the document. Two workers analyzing the first half and the second half of a long article are doing genuinely different work on genuinely different inputs. In *vision*, with the design we had, the workers are all looking at the same input and pretending to specialize. It is theater, not work-splitting.

Both errors compound. The text splitter cannot intelligently decompose a real user question because it has not seen the image. And even if it could, asking different questions about the same image gives no savings. The whole vision-by-sub-question architecture is broken from both ends.

I told this to Claude, plainly. Claude said yes, both are right, I was fooling myself. Good. We agreed the design was wrong. Now what.

---

## Cutting the photo

The fix came to me almost immediately, because once you see the problem you also see the only honest answer. **If we want real parallelism in vision, we have to give each worker a different *piece of the photo*, not a different question about the same photo.** Not a different category. A different region of pixels.

Picture the photo as a square. Cut it into four quadrants — upper-left, upper-right, lower-left, lower-right. Hand the upper-left to one worker, the upper-right to a second, the lower-left to a third, the lower-right to a fourth. Each worker now has only a quarter of the original image. Each worker runs its little vision model on its quarter. Each worker produces a text description of *only what is in its quarter*. Now the work is genuinely parallel: each worker is processing one-quarter of the pixels, doing one-quarter of the compute, and only seeing one-quarter of the scene. Add the four answers together at the end, and you have a description of the whole.

This is real work-splitting. It is the same idea as splitting a long document into chunks for text — each worker only sees its chunk, each worker only does the work for its chunk. The text-domain trick translates exactly into the image domain, except instead of cutting the document into paragraphs you cut the image into rectangles.

The Queen at the top does not need to *understand* the image to do the cutting. She does not need a vision model. She just needs a tiny program — a few lines of Python — that takes an image and splits it into four equal pieces. That program is not an AI. It is just arithmetic. Cut the width in half. Cut the height in half. Hand out the four resulting rectangles. No model reasoning is required.

This solves both fallacies at once. The split is not based on the content of the image, so the splitter doesn't need to see it. And each worker sees only its own piece, so the work is genuinely smaller.

But there is a problem.

The dog in the photo is curled up under a table. Suppose the cut goes right through the middle of the dog. Now the upper-left quarter contains the dog's head and the upper-right quarter contains the dog's tail. Neither worker can see a whole dog. The head-worker says *"I see the front half of a brown furry creature, maybe an animal, maybe a coat draped over something."* The tail-worker says *"I see the back half of a brown furry creature, maybe an animal."* Neither worker can recognize that what they are seeing is a dog, because neither worker is seeing the whole dog. The combiner upstairs would have to figure out from the two text descriptions that they are the same animal, sliced down the middle by the grid. That is a hard text-reasoning task and it might fail — the combiner might decide they are two different animals.

The fix is also obvious once you see it. Cut the image *twice*, with the second cut offset by half a tile. The first set of cuts goes at zero, fifty, and one hundred percent of the width and height. The second set of cuts goes at twenty-five, seventy-five percent of the width and height. The second grid is shifted half a tile relative to the first. Anything that gets sliced in half by the first grid is whole somewhere in the second. The dog under the table — split in half by the first cut — is sitting fully inside one of the second-grid tiles. That second-grid worker reports a complete dog. Even if the first-grid workers get confused, the second-grid worker does not.

You give up nothing for this — it is more workers, and workers are cheap on a hive of small cheap models. You gain certainty: nothing important is lost at a boundary, because every part of the image is photographed twice with two different framings, and the worse framing is rescued by the better one.

I sat back. This is real. This is genuine parallelism for vision. We were not fooling ourselves anymore.

---

## What about the combiner?

But there was a new problem, and this one I almost missed.

The combiner at the top — the Queen who has to take all the workers' text descriptions and merge them into one answer — has a hard job in this architecture. She has eight tiles' worth of descriptions in front of her, all in text, none of them with a picture attached. She has to figure out, from text alone, *where* in the original image each tile came from, *what* relationships exist between things in different tiles, and which detected objects in two different tiles are actually the same thing seen twice.

For example: the upper-left tile worker says *"I see a wooden table with plates and the lower legs of two people."* The upper-right tile worker says *"I see the upper bodies of two people sitting at what appears to be a wooden table edge."* The combiner has to recognize that *these are the same two people, the same table*. Not four people. Not two tables. The combiner has to do this kind of merging hundreds of times, across all eight tiles, in pure text, while remembering the spatial layout of the grid in her head.

That is a hard text-reasoning task. Pure text models are not good at spatial reasoning. They confuse left and right. They forget which tile is adjacent to which. They invent connections that are not there. With a powerful enough text model — a frontier model, the kind big labs run — it can be made to work. But on a hive of cheap small models running on consumer hardware, the combiner is one of the smallest, cheapest models in the system, and asking it to do precise spatial reasoning in pure text is asking too much.

I felt this for about thirty seconds. Then the second piece fell into place, and it was the piece that turned the whole thing into a chapter.

---

## Give the boss a low-resolution view

Here is what I said to Claude, almost word for word:

> *"We do not need the combiner to be very smart. We give him a reduced photo. Mechanically. Very easy. We have a tool — another little Python program — that takes every second pixel and throws away the others. It throws away half the resolution. It does this also to the lines, not just the columns. Every row 1, 3, 5, ... it keeps. Every row 2, 4, 6, ... it throws away. Same for the columns. The result is a copy of the photo with one-quarter the pixel count, half the linear resolution. We give this reduced photo to the boss model. Now the boss does not need to work hard — the photo is small, there are very few details, the boss can see at a glance: oh, it's a dog. And on the other hand, if you want to see exactly what the dog's collar says, you find it in the text reports from the workers. Not complicated 3D understanding of the world. Just text."*

When Claude saw what I meant, both of us understood at the same moment that we had stumbled onto something a lot bigger than a fix for a vision test. Let me explain why.

In this new design, the Queen at the top has *two* sources of information about the photo:

**Source one — her own eyes.** She has a low-resolution version of the whole photo, and she runs her own little vision model on it. The low-resolution version is small enough that vision is essentially free for her — there are not many pixels to process. What she sees is the *gestalt* of the scene. *There is a dog. There is a table. There are four people. There is a chalkboard. There is a clock. There is sunlight coming from the right. There is an awning with text on it that I cannot quite read.* She sees *where everything is*, but not *what the small details are*.

**Source two — the workers' text reports.** The workers each looked at a high-resolution tile of one piece of the photo. They each produced a detailed text description of their tile. *"In tile A2 I see a chalkboard with text reading 'Espresso €2.50.'"* *"In tile B1 I see a brown dog with a red collar; the collar has a tag reading 'BUDDY.'"* *"In tile A1 I see a clock with hands pointing to 4:15."* They saw *what the small details are*, in places, but no single worker saw the whole scene.

The combiner's job is now trivial. She does not have to reason in pure text about which tile is adjacent to which. She does not have to figure out spatial relationships from grid coordinates. She *already sees the spatial layout*, directly, in her own little eyes, on her low-resolution image. She just takes the workers' detailed text reports and decorates her gestalt map with them.

*"I see a dog at this position in my low-resolution view. The worker who looked at that position reports the dog's collar reads BUDDY. So in my final answer, I will say: there is a dog named Buddy under the table."*

*"I see a chalkboard at this other position. The worker who looked there reports it says Espresso €2.50. So in my final answer: there is a chalkboard listing espresso for €2.50."*

She is integrating, not reasoning. She is doing what a manager does when she has a map of a city in front of her and her field reporters are calling in details from each neighborhood. She does not need to figure out where each neighborhood is — she has a map. Her job is to put the reports on the right spot on the map. That is a much, much easier job than reasoning about adjacency in pure text.

This is exactly how human vision works, by the way. You do not look at a busy café scene and process every single pixel of every single object at full sharpness. Your eye has a tiny high-resolution patch in the very center — the fovea — about the size of your thumbnail at arm's length. Everything outside that patch is peripheral vision, which is *low-resolution*. Your brain uses peripheral vision to keep a *map* of the whole scene, and your fovea zooms in on one detail at a time. You look at the dog for a fraction of a second, your fovea reads the collar, your brain stores the result on the spatial map maintained by your peripheral vision, and your eye darts to the chalkboard. Two-tenths of a second on the chalkboard, fovea reads the price, brain stores it on the map, eye darts to the clock. You see the entire café in about two seconds, and your brain combines a hundred tiny foveal observations into one coherent understanding using the peripheral map as the integration substrate.

The Queen at the top of our hive *is the peripheral vision*. The workers at the bottom *are the foveas*. The integration that happens at the Queen *is what the brain does* — placing detailed observations onto a coarse spatial map. We were not just designing a vision system for a hive of computers. We were rediscovering the architecture biology already settled on after a billion years of evolution, on a different substrate.

---

## Every level does the same thing

Once you see the principle, you also see that it does not stop at one level. Every layer of the hive can do the same trick at a different scale.

The top of the hive — call her the RajaBee — gets the original full-resolution photo. She makes herself a low-resolution version (every other pixel, every other row) and runs her own vision model on it. She has the gestalt of the *whole photo*. Then she takes the original full-resolution photo and cuts it into four quadrants. She does not send these as tiles to workers — she sends them as *sub-photos* to her sub-coordinators, the GiantQueens. Each sub-photo is a quadrant of the original, at the original's full resolution.

Each GiantQueen now has a sub-photo that is one-quarter the area of the original but at the same per-pixel sharpness. She does *exactly what the RajaBee just did*. She makes her own low-resolution version of her sub-photo, runs her vision model on it, sees the gestalt of *her quadrant*. Then she cuts her sub-photo into four sub-sub-photos and sends them down to *her* sub-coordinators, the DwarfQueens.

Each DwarfQueen does the same thing again. Low-resolution view, vision pass, then cut into smaller pieces and dispatch to the workers below. The workers at the bottom of the tree finally get small tiles — one-sixteenth, one-sixty-fourth of the original area — at full per-pixel resolution. They run their tiny vision models on their tiny tiles and report back text descriptions to their parent DwarfQueens.

The text reports flow upward. Each DwarfQueen takes her workers' text reports plus her own gestalt view of her sub-sub-photo and writes a paragraph that integrates both. The paragraph flows up to her parent GiantQueen. The GiantQueen does the same — she takes her DwarfQueens' paragraphs plus her own gestalt view of her sub-photo and writes a higher-level paragraph. That paragraph flows up to the RajaBee. The RajaBee takes her GiantQueens' paragraphs plus her own gestalt view of the whole photo and writes the final answer to the user.

The hive is recursive. Every level does the same operation: see the gestalt of my region in low-resolution, dispatch the high-resolution sub-regions to my children, get back text reports, integrate them onto my own spatial map, write a paragraph, send it up. The only difference between levels is the *scale* of the region they are responsible for. The RajaBee is responsible for the whole picture at the coarsest zoom. The Workers are responsible for tiny tiles at the finest zoom. Every tier in between is responsible for some intermediate zoom, in some intermediate region.

Adding more levels to the hive just adds more zoom levels. The principle does not change with depth. The hive can be five levels deep or fifty levels deep — every level is doing the same thing.

---

## The same trick, in one dimension

Here is where it stopped being a clever fix for a vision test and started being something a lot bigger.

The principle is not really about *images*. The principle is about *sub-sampling the input*. You take a high-fidelity input and you make a coarse copy of it by mechanically throwing away most of the data. You give the coarse copy to the parent and the high-fidelity full version, sliced into pieces, to the children. The parent uses the coarse copy to keep a *map*; the children use their high-fidelity slices to find the *details*. The merge happens at the parent by placing the children's text reports onto the parent's coarse map.

Once you state it that way, you can see immediately that the same trick works on *sound*. Sound is a one-dimensional signal — a function of time and nothing else. A microphone records the air pressure in a room as a single number, forty-four thousand times a second, and the resulting sequence of numbers is the entire signal. There is no width and no height — just one axis, time, with one value at each moment. Where an image has *two* dimensions to sub-sample (width and height), sound has *one*. But the sub-sampling principle still works, exactly the same way, on the one axis it has.

Take a high-fidelity audio recording — say, an hour of conversation in a restaurant, recorded at forty-four thousand samples per second. Take every other sample and throw the rest away. Now you have a recording at twenty-two thousand samples per second — half the fidelity, half the file size, all the same total duration. The low-fidelity version still contains the *gestalt* of the sound: the rhythm, the speaker presence, the rough spectrum, the broad shape of the music or the conversation. You could not transcribe a whisper from it, but you can hear *that* there is a whisper, and *where* in the timeline the whisper happens. Just like the low-resolution image contains the gestalt of the scene without the small details.

Now take the original full-fidelity recording and cut it into one-second slices. Hand each slice to a worker. Each worker runs its tiny audio model on its one-second slice and produces a text description of what it heard: *"In this second I heard a male voice say 'hello.'"* *"In this second I heard a glass clink against a plate."* *"In this second I heard a child laugh."* The workers each only process one second of full-fidelity audio — genuinely smaller work — and produce text observations that sit on a timeline.

The parent at the top has a low-fidelity copy of the *whole* recording, runs her tiny audio model on it, and hears the gestalt: *"two people talking, occasional background clink of dishes, a child somewhere in the background, soft music with a steady rhythm, ambient noise consistent with a restaurant."* She has a *temporal map* — what the conversation feels like at each moment of its duration — without knowing exactly what was said.

The merge: she takes her temporal map and decorates it with the workers' text reports. *"At second 12, I hear that something happens — and the worker who listened to second 12 reports it was a male voice saying 'hello.' So at second 12, a man said hello."* *"At second 17, my low-fidelity version makes me think a child reacted to something — and the worker who listened to second 17 reports a child laughing. So at second 17, the child laughed."*

Same recursive structure. Same low-fidelity-at-parent, high-fidelity-slices-at-workers, text-reports-up, integration-at-each-level. The math is identical to the image case, just on one axis instead of two. The biology is identical too — this is how the cocktail party effect works in human hearing. You can pick out one conversation from a noisy room because your brain is doing the same trick: a low-fidelity scan of the whole acoustic scene, plus high-fidelity attention on one source at a time, integrated into one coherent perception.

So we now have the principle in **one** dimension (sound) and **two** dimensions (image). The next chapter scales it to three and four, and that is where the story gets really interesting — because three dimensions is the actual physical world the hive is moving through, and four dimensions is the physical world *changing over time*. But before we get there, I want to take one paragraph to talk about why any of this matters to you, the reader, who may not care at all about how a hive of small AIs reasons about pixels.

---

## Why should YOU care?

Skip this section if you are technical and want to keep going. If you are not technical, this section is for you.

What we just described is the answer to a question that has been quietly hovering over distributed AI for a long time: *can you do real perception — real seeing, real hearing — on a swarm of cheap small computers, instead of on one giant expensive computer in someone else's data center?*

Until tonight, the honest answer was *we are not sure*. There were techniques for doing it on one big machine, and there were techniques for distributing parts of one big model across several machines if you had expensive networking between them, but there was no good answer for *one tiny model per cheap machine, many cheap machines, no special networking*. The kind of setup an ordinary person could build in their living room. The kind of setup a hospital could build in its building. The kind of setup a small country could build for itself.

Now there is an answer. Cut the input mechanically along whatever axes it has. Send pieces to many small machines. Each small machine looks at its piece. Send text observations back. Combine them at each layer of the tree using a coarse view of that layer's region. It does not require expensive hardware. It does not require fast networking between the machines. It does not require any single machine to be smart. It works because *the architecture itself does the integration*, and because perception is doing the easy job (what is roughly here) while text is doing the easy job (what the details are), and the hard job (combining them) is done by simply placing one onto the other.

Concretely, in just the two dimensions covered in this chapter — sound and image — this means:

- **A hospital** can run image analysis on its medical scans and audio analysis on its dictation recordings and intercom conversations, all on the office computers it already owns, without sending a single byte to an outside service.
- **A small business** can analyze every photo a customer uploads and every voice message a customer leaves on machines costing a few hundred dollars each, instead of paying per-image and per-minute fees to a cloud service.
- **A country with no large AI industry** can build a sovereign perception system out of consumer laptops, give it to its hospitals and police forces and schools, and not depend on any foreign company to keep it running.
- **A research lab on a slow internet connection** can analyze high-resolution photos and recordings locally on whatever hardware it has, instead of waiting hours to upload each one.

That is just the first two dimensions. The third and fourth — which the next chapter covers — are even bigger, because they are the dimensions of the physical world itself, and they unlock the possibility of swarms of distributed perceivers that can map a place, navigate it, and track what moves through it, all without phoning home to any central server.

---

## What I learned tonight

I want to close this chapter by saying something honest about how this discovery happened, because I think the *how* is part of what I am trying to teach in this book.

I did not set out to discover a unifying principle for distributed perception. I set out to plan a small test for a small project. I asked a clear question and got a clear answer, and then I noticed the answer was wrong in two specific ways, and instead of patching the answer I sat with the wrongness for one more minute than I would normally have. In that minute, the right answer arrived — first as a fix for one of the two errors (cut the photo into pieces), then as a refinement on the fix (cut it twice with an offset to handle boundaries), then as a deeper insight about who needs to see what at which resolution (give the boss a low-resolution view), then as a recursive structure that scales to any depth, then as a generalization across sound and image, and (in the next chapter) as an answer to questions about three- and four-dimensional perception that I have been hand-waving for years.

None of these steps was hard, individually. Any of them, on their own, is a thirty-second observation. The whole chain took maybe forty minutes. Forty minutes from "I am about to approve a broken design" to "I think I just found something as big as the original idea this whole book is about."

The thing that made it work was *not* being smart. The thing that made it work was *refusing to fool myself*. When the design Claude gave me felt wrong, I stopped and named exactly what was wrong, even when I could not yet see how to fix it. When I saw how to fix it, I asked whether the fix had created a new problem, and when it had, I sat with the new problem instead of declaring victory. When the new problem dissolved into a deeper insight, I asked whether the insight generalized, and it did. Every step was a small act of *not pretending the previous step was good enough*.

The lesson, if there is one, is this. Most of the things you build will be slightly wrong in ways that are easy to ignore. The ones that turn into discoveries are the ones where you decide not to ignore the wrongness. Almost everything else takes care of itself.

The chapter you just read is the result of one evening of refusing to look away. The Queen at the top of the hive *does not see the photo herself* until she has a low-resolution copy in her own little eyes — but the principle of how she sees it was waiting for one of us to refuse to accept the broken design. The same principle had probably been waiting for years, in plain sight, in every textbook on image pyramids and every research paper on hierarchical attention. It just took someone willing to ask, in plain words, *what would it mean if you did this with cheap small models on cheap small machines, instead of with one giant model on one giant machine.*

That is the question this whole book is about. And the answer just got bigger.

In the next chapter, we add a third dimension and then a fourth, and we go from a hive that can hear and see to a hive that can map a physical place and watch the things moving through it.

---
