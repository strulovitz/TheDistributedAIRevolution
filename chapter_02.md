# Chapter 2: The Problem — Cloud AI vs. Local AI

Before we can understand the Beehive of AI and why it matters, we need to take an honest look at the world as it stands today. Because right now, when it comes to artificial intelligence, you basically have two choices — and both of them have a pretty significant catch.

Think of it like the housing market. You can either rent a luxury penthouse apartment that costs a fortune and where the landlord can walk in whenever they want... or you can live in a cosy garden shed that you own outright but that, frankly, doesn't have room for a proper kitchen. What most people actually want — a solid, affordable home they own — doesn't really exist yet. That's the gap we're here to talk about.

## The Cloud Giants: Powerful, But at a Price

When most people think of AI today, they think of ChatGPT, Google Gemini, Claude, Grok, and their cousins. Behind every response, there's an almost incomprehensible amount of infrastructure: warehouses the size of football fields packed with hundreds of thousands of specialised chips. OpenAI, Google, Microsoft, and Anthropic have collectively invested hundreds of billions of dollars. Elon Musk's xAI famously assembled 100,000 GPUs in Memphis in under 100 days — and that was considered fast.

The result is genuinely impressive. These models can write essays, code software, analyse legal documents, pass medical exams. For many tasks, they perform at or near human expert level.

But here's the catch: none of this comes free.

You pay $20/month as a regular user, and much more for business plans. As a developer, you pay per word processed — and a busy application can cost thousands of dollars a month. Beyond money, there are subtler problems:

- **Your data.** Every prompt travels to a server you don't control. Lawyers, hospitals, and defence contractors can't use these services for sensitive work — "trust us" isn't good enough in regulated industries.
- **Censorship.** Cloud AI providers have to serve billions of users across hundreds of countries. The result is sometimes-maddening refusals that mean the AI isn't fully under your control — it's under theirs.
- **Fragility.** When OpenAI's servers go down, every product built on top of them goes down too. You're entirely dependent on one company's infrastructure, pricing, and continued existence.

## The Home Alternatives: Free, But Limited

In the last couple of years, something remarkable happened. Models like Meta's Llama, Google's Gemma, DeepSeek, and Qwen are available for anyone to download and run locally — free, forever, no subscription, no data leaving your house. Tools like Ollama and LM Studio make setup take about 10 minutes.

The magic ingredient is quantisation — a way of compressing a model so it fits on consumer hardware. A model that originally needed 40GB of storage can be squeezed to 5 or 6GB, still performing surprisingly well. A model trained on a supercomputer cluster worth tens of millions of dollars can now run on a decent gaming PC.

The advantages are real. The cost is pennies per hour in electricity. Your data never leaves your machine. Nobody can shut it down or censor it. You own it completely.

So why isn't everyone using local AI?

Because of one fundamental limitation: a single GPU can only run models up to a certain size. The largest you can comfortably run at home has roughly 70 billion parameters — about 10 to 15 times smaller than the models powering GPT-4 or Claude Sonnet. For everyday tasks local AI is perfectly capable, but for serious multi-step reasoning or demanding professional work, it starts to struggle.

## The Gap in the Middle

On one side: extraordinarily powerful cloud AI that costs serious money, raises privacy concerns, enforces restrictions, and creates dangerous dependency on a handful of corporations. On the other side: capable local AI that is free and private, but significantly less powerful. In between? Almost nothing.

This isn't for lack of trying.

**Petals** pitched itself as "BitTorrent for AI" — splitting a model's layers across many computers, like floors of a building distributed across different machines. Brilliant idea. Didn't work. The problem: at every layer, the output is a massive tensor — hundreds of megabytes of numbers that need to travel over the internet to the next machine. Over a fast home connection, one transfer takes a second or two. Multiply by 32 layers and you're waiting a minute per response. Unusable.

**EXO** tried the same layer-splitting approach over local networks. Same wall. The tensor data is simply too large.

**AI Horde** is a volunteer network where people donate their GPU time. Lovely community effort — but it doesn't actually combine power. One volunteer machine handles your entire request on its own. It's a lending library, not a factory.

All of these projects share the same fatal assumption: that you solve this problem by splitting the model itself across machines. But the data flowing between those pieces is enormous, and the internet isn't fast enough.

## What If There Was a Third Way?

All the failed attempts were trying to solve the same problem the same way. Split the model. Distribute the layers. Make many computers pretend to be one giant computer.

But what if that's the wrong way to think about it entirely?

What if, instead of splitting the AI model across many computers, you split the *work* itself?

What if, instead of sending massive tensors across the internet, you only sent text — a question here, an answer there, a few hundred bytes instead of a few hundred megabytes?

What if each computer didn't need to know what the others were doing, because each one was handling a completely independent piece of the puzzle — running its own complete AI model, on its own hardware, at full speed?

That's a different kind of distributed AI. Not a distributed model, but a distributed mind. Not a factory floor where each worker handles one bolt on the same car, but a beehive — where thousands of bees, each doing their own complete job, together produce something none of them could make alone.

In Chapter 3, we'll see exactly how that works.
