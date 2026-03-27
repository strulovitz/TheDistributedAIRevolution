# Chapter 7 — From Zero to Beehive: Anyone Can Set This Up

I have made a lot of promises in this book. I told you that anyone can participate. I told you that you do not need technical skills. I told you that setting up your computer to earn money — or setting up your organization to use its own idle machines for AI — is something any person can do.

Now I am going to prove it.

This chapter walks you through the entire setup, from a computer that has nothing installed to a fully functioning beehive. I am going to do it in plain language, with no assumed knowledge. If you have ever installed a program on your computer — Spotify, Zoom, Chrome, anything — you can do this.

And if you have never installed a program on your computer, that is okay too. I will tell you what to click.

---

## Before We Start: What You Need

Let me set your expectations clearly.

To participate as a **Worker Bee** (earning money with your idle computer), you need:

1. A computer. Windows, Mac, or Linux. Made in the last five or six years. That is it.
2. An internet connection. Any speed is fine — the system sends small amounts of data, not streaming video.
3. About thirty minutes for the initial setup. After that, it runs itself.

To participate as a **Beekeeper** (submitting AI jobs), you need even less:

1. A web browser. Any browser. Chrome, Firefox, Safari, Edge — they all work.
2. An account on the BeehiveOfAI website.
3. That is literally it. There is no software to install.

To set up a **Queen Bee** (running a hive), you need more — powerful hardware, a bigger AI model, and some understanding of costs. I will cover the queen setup later in this chapter, but honestly, most people should start as a worker bee and graduate to queen only when they understand the system well and are ready to invest.

To set up the **Private Mode** (for an organization), you need one person with basic IT skills and access to the organization's computers. I will walk through that too.

Let us start with the role that ninety percent of readers will care about most: the Worker Bee.

---

## Setting Up as a Worker Bee

This is the simplest path, and I am going to walk you through it as if we were sitting side by side at your desk.

### Step 1: Get an AI Model Running on Your Computer

Before your computer can do AI work, it needs an AI brain. Remember from Chapter 3 — there are free programs that let you run AI locally. The one I recommend for most people is called **Ollama**.

Here is how to install it:

**On Windows:**
- Open your web browser and go to ollama.com
- Click the download button
- Run the installer — just click "Next" a few times, the same way you install any program
- When it is done, Ollama is running in the background. You will see a small llama icon in your system tray (the little icons in the bottom-right corner of your screen)

**On Mac:**
- Open your web browser and go to ollama.com
- Click the download button
- Open the downloaded file and drag Ollama to your Applications folder
- Launch Ollama. It appears as a small icon in your menu bar at the top of the screen

**On Linux:**
- Open a terminal (the text command window) and type: `curl -fsSL https://ollama.com/install.sh | sh`
- Press Enter. It installs itself. Done.

Now you need to download an AI model. This is the "brain" your computer will use. Open a terminal or command prompt (on Windows, search for "Command Prompt" in the Start menu) and type:

```
ollama pull llama3.2:3b
```

Press Enter. Your computer will download the model — it is about two gigabytes, so it might take a few minutes depending on your internet speed. When it finishes, you have a fully functional AI brain on your computer.

Want to test it? Type:

```
ollama run llama3.2:3b
```

Ask it anything. "What is the capital of France?" If it answers "Paris," your AI is working. Press Ctrl+D (or type `/bye`) to exit.

That is it. Step 1 is done. Your computer now has an AI brain.

**A note about model choices:** The llama3.2:3b model is small and fast — perfect for getting started. If your computer has a good graphics card with 8 or more gigabytes of memory, you can try a larger model like `llama3.1:8b`, which is smarter but slower on modest hardware. The beauty of this system is that you can change your model anytime. Start small, experiment later.

**What if you prefer a different AI program?** Ollama is my recommendation because it is the simplest, but the system works with several others too. **LM Studio** gives you a visual interface with windows and buttons — nice if you do not like typing commands. **llama.cpp** is a lightweight option that uses fewer resources. **vLLM** is the fastest option for powerful Linux machines. The HoneycombOfAI software detects which one you have installed and works with it automatically. You do not need to learn multiple programs — just pick one.

---

### Step 2: Download and Install HoneycombOfAI

HoneycombOfAI is the software that turns your computer into a Worker Bee (or a Queen Bee, or a Beekeeper — but we are setting up a Worker Bee right now).

Go to the HoneycombOfAI page on GitHub: **github.com/strulovitz/HoneycombOfAI**

You will see a big green button that says "Code." Click it, and then click "Download ZIP." Save the ZIP file somewhere on your computer and extract it (right-click, "Extract All" on Windows, or double-click on Mac).

Inside the folder, you will find the program and all its files.

**If you are comfortable with the command line**, you can also clone the repository:

```
git clone https://github.com/strulovitz/HoneycombOfAI.git
```

Either way, you now have the software on your computer.

---

### Step 3: Install the Requirements

The software needs a few supporting pieces to run. Open a terminal or command prompt, navigate to the HoneycombOfAI folder, and type:

```
pip install -r requirements.txt
```

This installs the necessary pieces automatically. It takes about a minute.

**If you get an error** that says `pip` is not recognized, you need to install Python first. Go to python.org, download the latest version, and during installation, make sure to check the box that says "Add Python to PATH." Then try the pip command again.

---

### Step 4: Launch the Software

Now comes the moment of truth. You have two options for running HoneycombOfAI — a graphical interface with windows and buttons, or a text-based terminal interface. Both do exactly the same thing.

**Option A — The graphical interface (recommended for most people):**

```
python gui_main.py
```

A window opens. You see three cards — Worker Bee, Queen Bee, and Beekeeper. Click **Worker Bee**. A dashboard appears showing your worker status, ready and waiting.

Before you start working, click the **Settings** button (the gear icon). Here you can:

- Set the address of the BeehiveOfAI server you want to connect to
- Choose your AI backend (the software automatically detects if you have Ollama, LM Studio, or others installed)
- Test your connection to make sure everything works

Click "Test Connection" — if you see a green checkmark, you are ready.

Now click **Start**. Your computer is now a Worker Bee. It connects to the hive, announces itself as available, and waits for tasks. When a task arrives, you will see it in the activity log on your dashboard. Your computer processes it, sends back the result, and waits for the next one.

That is it. You are earning.

**Option B — The terminal interface:**

```
python honeycomb.py --mode worker
```

Text scrolls across your screen showing the worker connecting and waiting for tasks. When tasks arrive, you see them being processed in real time. It is less pretty but works exactly the same.

---

### Step 5: Walk Away

This is the most important step, and it is the easiest: do nothing.

Leave the software running. Go about your day. Go to work. Watch a movie. Sleep. Your computer handles everything. When tasks come in, it processes them. When there are no tasks, it waits quietly using almost no resources.

You can check in anytime — the dashboard shows how many tasks you have processed, how much you have earned, and how long your computer has been working. But you do not have to check. The whole point is that this runs without you.

---

## Setting Up as a Beekeeper

If you want to submit AI jobs rather than process them, your setup is even simpler: there is no software to install at all.

### Step 1: Visit the Website

Open your browser and go to the BeehiveOfAI website. The address depends on which marketplace you are using — if it is the main public marketplace, it will be at **beehiveofai.com**. If someone has set up their own marketplace (remember, the software is open source — anyone can), they will give you their address.

### Step 2: Create an Account

Click "Sign Up." Enter your details. You will receive a verification code via text message to confirm you are a real person. Enter the code, and your account is active.

### Step 3: Submit a Job

Click "Submit a New Job." Describe what you need done — in plain language, the way you would explain it to a smart assistant. "Analyze these customer reviews and categorize them by sentiment." "Write product descriptions for these twenty items." "Summarize these research papers."

Attach any supporting files or information. Choose a hive from the available options (look at their reputation and ratings). Set your budget. Click Submit.

### Step 4: Wait (Briefly)

Depending on the size of your job, results come back in seconds to minutes. You will get a notification when your job is complete. Download the results, review them, and rate the hive's work. Your rating helps other beekeepers choose good hives, and it helps good queens build their reputation.

That is the entire beekeeper experience. No software. No technical knowledge. Just a browser and a task.

---

## Setting Up as a Queen Bee

Now we get to the role that requires real investment — both in hardware and in understanding. If you are reading this section, you are probably the kind of person who sees opportunity and wants to build something. Good. But read carefully, because the queen role has real costs, and understanding those costs is the difference between profit and loss.

### The Hardware Decision

As I explained in Chapter 4, there are three tiers of queen hardware:

**Tier 1: A Powerful Home Computer.** A desktop with a high-end graphics card — or even better, two graphics cards. This lets you run a larger, smarter AI model than what fits on a single card. The cost is significant but within reach for someone making a serious investment. Think of it as buying equipment for a small business, because that is exactly what it is.

**Tier 2: Specialized AI Hardware.** Products like NVIDIA's DGX Spark — compact devices designed specifically for running large AI models. These are a step up in both capability and cost, but they give you access to dramatically more powerful AI models, which means your hive produces dramatically better results.

**Tier 3: Cloud GPU Servers.** Renting powerful machines from providers like RunPod, Lambda, or similar services. You do not buy the hardware — you rent it by the hour. This gives you the most power but requires careful cost management. Remember my story from Chapter 4 about the $127 surprise bill? That is what happens when you rent powerful hardware and forget to watch the meter.

For a new queen, I recommend starting with Tier 1 — your own powerful computer. It has no ongoing rental costs, so your only expenses are electricity and the initial hardware purchase. You can always scale up later.

### Setting Up the Queen Software

The queen runs the same HoneycombOfAI software as the worker — just in a different mode.

Install a powerful AI model. For a queen, you want something bigger than what the workers are running. If your workers are running llama3.2:3b (3 billion parameters), your queen should be running something like llama3.1:70b (70 billion parameters) or even larger if your hardware can handle it. The queen's AI is what splits jobs into smart subtasks and combines results into polished final products — the quality of that AI determines the quality of everything your hive produces.

Launch the software in queen mode:

**Graphical interface:**
```
python gui_main.py
```
Then click the **Queen Bee** card. You will see the Queen Bee console — a dashboard showing the job board, subtask tracking, and activity log.

**Terminal:**
```
python honeycomb.py --mode queen
```

In the settings, configure your queen to connect to the BeehiveOfAI marketplace. Register your hive. Set your pricing. And then the queen software begins monitoring for incoming jobs.

### Building Your Hive

A queen without workers is a queen without a kingdom. You need worker bees to join your hive.

In the public mode, workers can discover your hive through the marketplace and request to join. You can accept or reject them based on their hardware capabilities and reliability. Over time, you build a team of workers you trust.

The key to a successful hive is curation. Accept workers with reliable hardware and good track records. Monitor quality — if a worker consistently produces poor results, remove them and accept someone better. Your reputation depends on the quality of your entire hive, not just your own AI. A single unreliable worker who produces bad results can drag down your ratings.

### Managing Costs

If you are renting cloud hardware (Tier 3), managing costs is critical. Here are the rules:

- **Know your hourly cost.** If your GPU server costs $2 per hour, you need to earn more than $2 per hour to be profitable. Simple math, but many people forget to do it.
- **Scale with demand.** When jobs are flowing in steadily, keep your servers running. When things are quiet, shut them down. Never leave a server idling with no work to process.
- **Start small.** Rent the smallest GPU that can run your target AI model adequately. You can always upgrade later when you have steady income to justify it.
- **Track everything.** Keep a spreadsheet. Revenue in one column, costs in the other. If the second column is ever bigger than the first for more than a day or two, something needs to change.

---

## Setting Up the Private Mode (For Organizations)

This is the section for IT managers, system administrators, or anyone tasked with setting up a distributed AI system inside an organization. The private mode is where BeehiveOfAI shines for organizations with sensitive data — hospitals, law firms, banks, government agencies, and anyone else who cannot send data outside their walls.

### What You Need

- **Computers.** You already have them — the desktops sitting on every desk, the workstations in every department. Any modern computer can participate.
- **A network.** Your existing internal network is fine. The computers just need to be able to talk to each other, which they already can if they are on the same office network.
- **One designated queen.** Pick the most powerful computer available — ideally one with a good graphics card. This will be your queen bee, the one that splits and combines tasks.
- **One person with basic IT skills.** This person does the initial setup. After that, the system runs itself.

### Step 1: Set Up the Website Internally

The BeehiveOfAI website runs on your internal network — not on the public internet. Download the BeehiveOfAI code from GitHub (github.com/strulovitz/BeehiveOfAI) onto one computer. This will be your internal marketplace server.

Follow the deployment guide (DEPLOY.md in the repository) to get it running. The short version: install Python, install the dependencies, run the server. It takes about fifteen minutes.

Once it is running, any computer on your internal network can access it by typing the server's address into a web browser. It looks and works exactly like the public marketplace — except it is only accessible from inside your building.

### Step 2: Install the AI Model on Every Computer

This is the most time-consuming step, but it is simple and repetitive. On each computer that will participate as a worker, install Ollama (or your preferred AI backend) and download the AI model.

For a large organization, your IT person can create a simple script that does this automatically on every machine. Install Ollama, pull the model, done. If you have two hundred computers, the script runs on all two hundred. It takes a few minutes per machine, and they can all run simultaneously.

### Step 3: Install HoneycombOfAI on Every Computer

Same process. Download HoneycombOfAI, install the requirements, configure each machine to point at your internal BeehiveOfAI server. Again, a script can automate this for hundreds of machines.

On the queen machine, configure HoneycombOfAI in queen mode with a more powerful AI model. On all the others, configure them as workers.

### Step 4: Start Everything

Start the workers, start the queen, and the system is live. Your internal departments can now submit AI jobs through the internal website, and the organization's own computers process them.

The data never leaves the building. Not one byte goes to the internet. The AI models run locally on each machine. The results stay on the internal network. Complete privacy, complete compliance, complete control.

### Step 5: Ongoing Operations

The beautiful thing about the private mode is that it needs almost no ongoing attention. The computers process tasks during idle hours — nights, weekends, lunch breaks. The software starts automatically when the machines are on and stops gracefully when someone needs to use the computer for their regular work.

The IT person might check in once a week to make sure everything is running smoothly. Update the AI models occasionally when better ones become available (they are always free). Add new computers to the worker pool when the organization buys new machines. Remove old ones when they are retired.

That is it. An organization's own AI processing network, running on hardware it already owns, processing data that never leaves the building, maintained by one person spending an hour a week.

---

## "But What If I Get Stuck?"

Setup is straightforward, but I know that technology can sometimes be unpredictable. A command might not work on your specific computer. An error message might appear that you did not expect. Something might not click the first time.

Here is what you do.

**Check the documentation.** Both HoneycombOfAI and BeehiveOfAI have README files with detailed instructions, troubleshooting tips, and common solutions.

**Check the GitHub Issues page.** If someone else has had the same problem, the solution is probably already there.

**Ask for help.** The project is open source, and open source projects thrive on community. Post your question on the GitHub Discussions page, describe what happened, and someone — maybe another user, maybe me — will help you figure it out.

**Use an AI assistant.** This might sound circular, but it is actually the most practical advice I can give. Copy the error message you are seeing, paste it into ChatGPT or Claude or any AI assistant, and ask "What does this mean and how do I fix it?" AI assistants are remarkably good at troubleshooting computer problems. After all, that is one of the things AI was made for.

**One thing Windows users must know.** If your BeehiveOfAI website is running on a Windows machine and Worker Bees or Queen Bees on other computers cannot connect — even though the website works fine in your own browser and you can ping the Windows machine — the almost certain culprit is Windows Defender Firewall. It silently blocks incoming connections to Python with no warning, no popup, nothing. The fix is to open PowerShell as Administrator and run:

```
New-NetFirewallRule -DisplayName "BeehiveOfAI Python" -Direction Inbound -Action Allow -Program "C:\full\path\to\your\python.exe" -Protocol TCP -Profile Any
```

To find your Python path, run `python -c "import sys; print(sys.executable)"` first. No restart needed — the rule takes effect immediately. Full instructions are in the TROUBLESHOOTING.md file in the BeehiveOfAI repository.

The point is: you are not alone. Even though this is a new project, the technologies it is built on — Python, Ollama, Flask, PyQt6 — are used by millions of people. Help is always available.

---

## The Demo Mode: Try Before You Fly

Maybe you are not ready to commit yet. Maybe you want to see the system in action before you install an AI model or connect to a real marketplace. That is completely reasonable.

HoneycombOfAI has a **Demo Mode** built in. When you run the software in demo mode, it simulates the entire process without needing any AI model installed and without connecting to any server. The worker pretends to receive tasks and process them. The queen pretends to split and combine. The beekeeper pretends to submit and receive.

It is a sandbox — a safe playground where you can explore the software, see how the interface works, understand the flow, and decide which role interests you — without installing anything beyond the basic software itself.

To try it:

```
python gui_main.py
```

In the settings, select "Demo" as your AI backend. No Ollama needed. No model download needed. Just click and explore.

When you are ready for the real thing, switch to a real AI backend and connect to a real server. The transition is one settings change away.

---

## A Real Setup, Start to Finish

Let me paint the complete picture by following one person through the entire process.

Maria is a graphic designer in Madrid. She has a decent desktop computer — nothing fancy, but it has a mid-range graphics card because she sometimes works with large image files. She read this book (or at least the first few chapters), and she wants to try being a Worker Bee.

**Tuesday evening, 7:00 PM:** Maria goes to ollama.com and downloads Ollama. The installer takes two minutes. She opens a command prompt and types `ollama pull llama3.2:3b`. The download takes four minutes on her home internet.

**7:10 PM:** She goes to github.com/strulovitz/HoneycombOfAI and downloads the ZIP file. She extracts it to her Documents folder. She opens a command prompt, navigates to the folder, and types `pip install -r requirements.txt`. It takes ninety seconds.

**7:15 PM:** She types `python gui_main.py`. The HoneycombOfAI window opens. She clicks Worker Bee. She goes into Settings, enters the server address, and clicks "Test Connection." Green checkmark. She clicks Start.

**7:17 PM:** Maria's computer is now a Worker Bee. Seventeen minutes from zero to earning.

She goes to make dinner. While she is cooking, her computer processes three text analysis tasks. She does not notice. She does not need to.

**Wednesday morning, 8:00 AM:** Before leaving for work, Maria glances at her dashboard. Her computer processed fourteen tasks overnight. She earned a small amount of Nectar. Not life-changing, but her computer was asleep — and it still earned money.

She leaves for work. The computer keeps working.

**A week later:** Maria checks her earnings dashboard. Her computer has processed several hundred tasks over the week, running mostly during the hours she was asleep or at her day job. The earnings are modest — enough for a nice dinner out. She considers leaving her computer on during the day while she is at work to increase her earnings.

**A month later:** Maria has earned enough to cover her internet bill. She upgrades her AI model to a slightly larger one — her graphics card can handle it — and her tasks per hour increase. She tells her friend Carlos about the system. He sets up his gaming computer as a worker too. His machine is faster and earns more, because his graphics card is more powerful.

That is the worker bee journey. From curiosity to earning in seventeen minutes.

---

## An Organization, Start to Finish

Now let me show you the same journey, but for an organization.

A medium-sized law firm in Chicago has three hundred computers — desktops on every lawyer's and paralegal's desk. The firm regularly handles document review cases where they need to analyze thousands of documents. Currently, they pay junior associates to read through documents manually, at $200 per hour. A large case might require two thousand hours of document review — $400,000.

The firm's IT manager, David, reads this book. He sees the private mode and has an idea.

**Monday, 9:00 AM:** David downloads BeehiveOfAI and HoneycombOfAI from GitHub onto the firm's internal server. He follows the deployment guide and has the internal website running by 10:00 AM.

**10:00 AM — 12:00 PM:** David writes a simple deployment script. It installs Ollama and downloads the AI model on a target machine. He tests it on three computers in the IT office. It works.

**After lunch:** David pushes the script to all three hundred computers via the firm's standard software deployment system (every organization has one — it is how they install updates on all machines). By 3:00 PM, Ollama and the AI model are installed on every machine in the building.

**3:00 PM — 4:00 PM:** David installs HoneycombOfAI on the same three hundred machines using the same deployment system. He configures the most powerful workstation in the server room as the queen. Everything points to the internal BeehiveOfAI website.

**4:30 PM:** David starts the queen and the workers. The internal distributed AI network is live. Total setup time: one working day.

**That evening, 6:00 PM:** Everyone goes home. Three hundred computers sit idle on their desks, connected to the internal network, waiting for work.

**The next morning:** A senior partner submits a test job through the internal website — "Analyze these fifty sample documents and flag any unusual liability clauses." The queen splits the job into fifty subtasks. The worker computers — the same machines that lawyers use for email during the day — process the documents in minutes. The results come back organized, categorized, and remarkably thorough.

The senior partner calls David. "How much did this cost?"

"Nothing," David says. "The computers were already here. The software is free. The AI models are free."

"And the data?"

"Never left the building. Not one word."

The partner is quiet for a moment. Then: "Can we do two thousand documents?"

David smiles. "We can do twenty thousand."

---

## What You Have Learned

Let me summarize what this chapter has shown you:

1. **Worker Bee setup takes about fifteen to twenty minutes.** Download Ollama. Pull a model. Download HoneycombOfAI. Install requirements. Launch. Done.

2. **Beekeeper setup takes about two minutes.** Visit the website. Create an account. Verify your phone. Submit a task. Done.

3. **Queen Bee setup requires real investment** — in powerful hardware, in a larger AI model, and in understanding your costs. But the software setup itself is no harder than the worker setup. The challenge is running a profitable operation, not installing software.

4. **Private mode setup takes about a day** for one IT person to configure for an entire organization. After that, it runs itself with minimal maintenance.

5. **Demo mode lets you try everything** without committing to anything. No AI model needed. No server connection needed. Just explore and learn.

6. **Help is always available.** Documentation, community forums, GitHub Issues, and AI assistants can all help you past any obstacle.

The technology works. The setup is simple. The barrier to entry is lower than almost any other way to earn money with your computer — because there is almost no barrier at all.

In the next chapter, I will show you something even more convincing than instructions: proof. Real tests, on real computers, over the real internet, with real results. Not hypothetical. Not simulated. Actually done, by me, and documented so you can see it with your own eyes.

