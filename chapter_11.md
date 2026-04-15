# Chapter 11 — How the Hive Senses the Simplest Things: Why One Number Becomes a Field

---

The simplest possible sensor produces one number. The temperature at this point is 22 degrees Celsius. The light level here is 340 lux. The concentration of carbon dioxide in this corner is 412 parts per million. The humidity is 47 percent. The air pressure is 1013 hectopascals. Nothing fancy. Nothing structured. One number at one moment at one place.

Most of the sensors an engineer can buy off the shelf, plug into a cheap microcontroller, and ship on a small drone tomorrow are of this kind. A chip the size of a fingernail, a few pins, a few dollars, and every few milliseconds it hands you a single number that describes what is happening in the physical world at the point where the chip is sitting. A temperature chip. A humidity chip. A light level chip. A gas concentration chip. An infrared thermometer. A radiation counter. A magnetic field sensor. An ultrasonic distance sensor. Each one is beautifully simple. Each one, **by itself, is almost useless.**

I want to begin the hive's perception arc with this case — the simplest possible sensor — because the simplest possible sensor is where the difference between a centralized AI brain and a distributed swarm is most visible. The more complicated the sensor (a camera, a microphone, a radar, a 3D scanner), the more the conversation gets dragged into arguments about model size and latency and compression and who has the better GPU. A single-value sensor is so simple that there is nothing to argue about in the sensor *itself*. All of the interesting properties come from the **distribution** of the sensors in physical space. And that is where the hive principle makes its strongest case — not later in the arc, when we get to sound and images and 3D maps, but right here, at the very beginning, with the dumbest kind of chip you can buy on Amazon for three dollars.

This chapter is about what happens when you take the dumbest kind of sensor in the world and spread a lot of them across a physical space. The answer is: something qualitatively different from any single sensor can ever produce, no matter how expensive or how precise that single sensor is. The answer is: a **gradient field**. And the gradient field is the thing the hive architecture was secretly designed to hold.

---

## Why one number is not enough

Pick any single-value sensor. Mount it on a single robot. Ask the robot a question like *"where is the source of the gas leak?"* or *"where is the hottest spot in this server room?"* or *"where is the brightest light in this dark warehouse?"* The robot points its one sensor at the air, at the wall, at the ceiling, and reads one number. That number tells the robot *what is happening right where the robot is, right now*. It does not tell the robot which direction the source is in. It does not tell the robot how far away the source is. It does not even tell the robot whether the reading is high or low compared to the rest of the building, because the robot has nothing to compare against.

For the robot to find the source with one sensor, the robot has to *move*. Take a reading. Walk three meters. Take another reading. Compare the two. If the second is higher, the robot is walking toward the source — keep going. If lower, turn around. Take another reading. Adjust. Wander. This technique has an ancient name for each sense — *chemotaxis* when the sensor is a gas sensor, *thermotaxis* when it is a temperature sensor, *phototaxis* when it is a light sensor. The Greek words are ancient because the principle is ancient. It is how bacteria find food. It works, slowly. In a simple room with one strong source, one robot can find the source in a few minutes. In a real environment with multiple sources, swirling air, partial obstacles, intermittent signal, and background noise, one robot wandering around gradient-climbing usually gives up before it finds anything useful.

This is the state of the art for solo-robot single-value sensing in 2026. It is fine for a research demo. It is nowhere near enough for a deployed application. A single nose is not a bloodhound. A single thermometer is not a heat map. A single light meter is not a map of the sun's position in a forest. **One number at one point is an observation; it is not perception.**

Now do the same thing with ten drones spread out over ten meters.

---

## Why many numbers spread out become a field

Imagine, instead of one robot with one sensor, **ten drones spread out across a ten-meter region, each with its own sensor, each reading the same physical quantity at the same moment.** Now you do not have one number. You have ten simultaneous numbers, taken at ten known positions in space, at the same instant. Ten numbers at ten positions is not a slightly bigger version of one number. It is a *qualitatively different* kind of information. **Ten numbers at ten positions is a gradient field.** The differences between the readings, divided by the distances between the drones, tell you which way the quantity is rising and how steeply.

A gradient field tells you the source direction **immediately**. Not after wandering. Not after hill-climbing. Not after trial and error. *Immediately*, from the very first set of simultaneous readings, before any drone has moved even one meter. The drone with the highest reading is closest to the source. The drone with the lowest reading is farthest. The vector from the lowest to the highest — projected forward — points at the source. You can solve in one observation what a solo robot has to solve by walking around for an hour. And as soon as the drones move slightly (even a normal flight maneuver) they re-sample, and the gradient updates, and you now have a gradient field that evolves over time, which is exactly the four-dimensional spacetime model of Chapter 14 applied to chemical, thermal, or photometric concentration.

**Distance between sensors normally costs you something.** It costs you latency. It costs you communication bandwidth. It costs you the difficulty of integrating observations made at different places. For every *other* sense in this book — sound, vision, mapping — the distance between sensors is a problem the hive principle has to *overcome*. The clever thing is that the principle handles it well.

For single-value sensors, distance between sensors is **the entire point**. Distance between sensors is *the information*. The hive does not have to overcome distance for these sensors — the hive *needs* distance, the more the better, and the more drones spread farther apart the easier the source-finding becomes. **This is the sense-class where the hive architecture stops being a workaround for the lack of one big machine and starts being strictly better than any single machine could ever be, no matter how smart.** A single brilliant AI in a data center cannot feel the temperature gradient across a cornfield in Iowa from California. A thousand cheap drones spread across that cornfield can, instantly. No amount of computation makes up for not being in the right place. **The information is a property of space, not a property of computation.**

That sentence is the core of this chapter, and I want you to remember it across every later chapter. *The information is a property of space, not a property of computation.* Once you accept that, every other sense the hive eventually develops — hearing, vision, 3D mapping, time-integrated perception, the vector mesh — becomes easier to think about, because you start to notice that *all of the senses* have a spatial component the hive is uniquely able to exploit. The single-value sensor chapter is just the case where the spatial component is *almost everything*, so the argument is cleanest.

---

## The Arduino ecosystem — every cheap sensor you can imagine

Before we talk about what any of this is *used for*, I want to show you how cheap and how ready the parts already are. This is not a science-fiction chapter. Every sensor in the following list can be bought for a few dollars, today, from any hobby electronics supplier, and wired to a twenty-dollar microcontroller by a teenager:

- **Temperature & humidity** — DHT11 and DHT22 chips (the popular beginner sensors), or the BME280 for better precision plus air pressure.
- **Air quality and specific gases** — the MQ-series (MQ-2 for smoke and LPG, MQ-7 for carbon monoxide, others for alcohol, methane, ammonia, hydrogen sulfide). For more sophisticated multi-channel gas sensing, the SGP30 measures several volatile organic compounds at once.
- **Distance** — HC-SR04 ultrasonic rangefinders, used in every beginner robotics project on earth.
- **Motion** — PIR (passive infrared) sensors detect human or animal body heat at ranges up to several meters, without any moving parts.
- **Orientation & acceleration** — the MPU6050 combines a 3-axis accelerometer and gyroscope so the drone knows its tilt, its rotation, and its acceleration.
- **Location** — GPS modules like the NEO-6M for outdoor work.
- **Light** — LDRs (light-dependent resistors) for dumb analog light measurement, BH1750 for a digital lux reading.
- **Sound intensity** — small microphone modules that report an envelope level without trying to interpret the sound (that is the *next* chapter's job).
- **Water quality, soil moisture, pH** — basic environmental monitoring sensors for agriculture and aquariums.
- **Specialty** — color sensors, fingerprint sensors, load cells, capacitive touch, magnetic field, Hall effect, UV, radiation counters.

You can put any of these on a drone for the price of a cup of coffee each. You can put *many* of them on the same drone. And the moment you spread those drones across a physical space, every one of these sensors becomes one channel of a distributed gradient field that no single-machine AI on earth can ever match, because the AI is in one building in California and your drones are in whatever space you put them in.

---

## Cortex and spine — the Pi plus the Arduino

**A small practical note on the hardware architecture of each drone,** because I do not want this to feel abstract. Each drone has two processors working together:

- A **Raspberry Pi** (or similar small single-board computer) serves as the drone's *brain*. It runs the vision and audio models that come online in later chapters, it runs the text-reasoning LLM, it holds a local piece of the swarm's collective memory mesh, it talks to other drones over the swarm's internal network, it makes the high-level decisions. The Pi is the *cortex*.
- An **Arduino** (or similar microcontroller) serves as the drone's *peripheral nervous system*. It is wired directly to every one of the cheap sensors listed above. It reads those sensors continuously — often at hundreds of samples per second — and hands the numbers up to the Pi over a simple serial link. The Arduino does not think. It does not reason. It does one job — *read sensors at a steady rhythm and forward the numbers* — and it does that job so fast and so reliably that the Pi can ignore the sensors completely and just listen for the Arduino's stream.

Nir described this split as *"CPU versus GPU of a complete computer."* That is the right spirit — two specialized coprocessors on one drone, each doing what the other would be terrible at. Strictly speaking, the cleaner biological analogy is **the cortex and the spinal cord**. Your cortex does not manage your individual muscle fibers or your individual pain receptors; your spinal cord does. The cortex receives *summaries* from the spinal cord ("the hand is hot," not "neuron 47,283 is firing at 34 Hz"), and makes strategic decisions ("move the hand") that the spinal cord then translates back into muscle-fiber-level commands. The Pi is the cortex. The Arduino is the spine. The real-time loop runs in the Arduino; the thinking runs in the Pi; the two talk through a narrow channel that carries only the information each needs from the other.

This matters because it shows that the hive drone is *already a hive in miniature*, even before it joins the larger swarm. Every drone has a cortex and a spine. Every swarm has a cortex (the RajaBee) and many spines (the workers). The fractal extends one level below the drone and one level above. **Every hive is made of hives, all the way up and all the way down.** The simplest sensors sit at the very bottom of that fractal, in the Arduino, handing their numbers up to the Pi, which hands its summaries up to the DwarfQueen, which hands its summaries up to the GiantQueen, which hands hers up to the RajaBee. Same architecture at every scale. The very first link in the chain is a three-dollar chip reading one number.

---

## The biology — every animal that smells, every animal that feels heat, every animal that sees light

Let me walk through a few of the biological systems that already do, on tiny brains, what we are now trying to do with cheap distributed sensors. Every one of these animals is solving the same problem the hive solves, and every one of them solves it by the same trick — spread the receptors across space, integrate the gradient at a central point.

**Mosquitos and CO2.** A female mosquito hunting for blood does not see her target and does not hear her target. She *smells* the target, from up to fifty meters away, by following the carbon dioxide that every mammal exhales with every breath. Mosquito olfaction can detect concentrations a few hundred parts per million above ambient. A flying mosquito samples continuously, the samples form a time-gradient as she moves, she climbs the gradient upwind toward the source. Once she lands on a human, she does not take a sip — she draws as much blood as her abdomen can physically hold, often three times her own body weight in one feeding, because finding a human is metabolically *expensive* (most flights end in failure) and *exploitation of a successful track has to be maximal to make the math work*. **This is a general rule the hive should adopt for rare-detection events: search is expensive, exploitation must be maximal.** When the swarm finds something rare, every drone within range should converge and document fully, not pass through and continue searching.

**Ants and pheromone trails.** An ant colony has no central planner. Each ant lays down a chemical trail when it walks, the trail evaporates over time, useful trails get reinforced because more ants follow and lay more chemical, useless trails fade. **The colony's collective memory of where to look is stored as gas concentrations in the environment itself.** The computation is happening in the air, not in any ant's brain. Ten thousand ants with brains the size of a grain of sand collectively map food sources in a square kilometer of forest, by smell alone, with no central server and no intelligence in any individual ant beyond *follow stronger trails, lay more trail when you find food*. The hive can do the same digitally.

**Salmon homing.** A salmon hatched in a mountain stream in Alaska migrates to the open Pacific, lives for years, and then returns *to the exact stream it was born in* to spawn — across thousands of miles, finding her way at the end by smelling her natal stream's specific chemical signature at parts-per-trillion sensitivity. She is distinguishing one stream from every other stream draining into the same ocean. A swarm of cheap noses spread across a watershed can do the same — distinguish one factory's effluent from another, track an oil spill back to its source vessel, identify which farm's runoff contaminates which downstream town.

**Moths and pheromone plumes.** A male silk moth can detect a single molecule of female pheromone on his antennae and follow the wind-borne plume of those molecules for several kilometers. He does this with about one hundred thousand neurons total — a brain ten thousand times smaller than a mouse's. His huge feathery antennae are *physically spread out* exactly so that the many receptors form a tiny gradient-detecting array on each antenna. The moth is, in a sense, a hive of one — his antennae are a little swarm of receptors with a central integrator.

**Bloodhounds.** Trained bloodhounds follow human scent trails several days old over many miles of mixed terrain. Their olfactory system has about three hundred million receptors compared to our six million, but the more important trick is *integration over the dog's own movement*. The dog builds a scent-gradient model as a trajectory through time and space, not as a snapshot at any single moment. A single-animal version of what the swarm does with many drones in parallel.

**Rattlesnakes and infrared pit organs.** A rattlesnake can detect the body heat of a mouse at several meters in total darkness, using two tiny pit organs on her face that function as thermal eyes. Each pit has a membrane whose temperature changes when infrared radiation from a warm body reaches it. The snake has *two* pits — the minimum required to triangulate direction — and she uses the tiny temperature difference between them to compute which way the warm object is. **Two thermal sensors spread across a few centimeters of snake face become a thermal vision system.** Spread the same sensors across ten meters of drone formation, and a thermal vision system becomes a thermal mapping system for an entire building.

**Sharks and electroreception.** Sharks have pits called *ampullae of Lorenzini* that detect electric fields in water. Every living animal generates a tiny electric field from its muscle and nerve activity. A shark swimming over a sandy ocean floor can detect a fish hiding under the sand because of the fish's electric field, even when there is no visual clue. **The shark's ampullae are a distributed electric-field sensor array**, the same architecture as everything else in this chapter. Put electric field sensors on underwater drones and you can detect hidden wiring in walls, electric cables under soil, active motors inside sealed compartments — anywhere electricity runs, which is almost everywhere in a modern facility.

The pattern across every one of these animals: **spread the receptors across distance, integrate the gradient at a central point, exploit successful detections fully, leave chemical or behavioral memory in the environment, follow time-gradients as well as space-gradients.** All five are things the hive principle is uniquely able to do at scale, on cheap hardware, on as many sensor modalities as we can afford.

---

## Temperature, specifically — two examples that make it click

Temperature deserves its own section because Nir pointed out two examples during the writing of this chapter that I want on the record, because they make the principle concrete in the specific case where a centralized AI cannot compete at all.

**Did a submarine pass here?** Imagine a swarm of small underwater drones floating at different depths in a specific patch of ocean. Each drone carries a cheap thermistor — a chip that costs pennies and reports water temperature to within a tenth of a degree. A submarine passing through that patch of water leaves a *thermal wake* — the water near its propeller is warmed, very slightly, by the mechanical work of the propeller spinning through it, and the warmed water persists for many minutes before diffusing away. A single thermometer in the ocean cannot detect this — the temperature change is too small and too localized. **A swarm of thermometers spread across a hundred meters of ocean, all reporting simultaneously, detects the wake immediately as a gradient anomaly**, tracks it to its source, and reconstructs the submarine's path through that patch of water hours after the submarine has left. No cameras. No sonar. No radio contact with any surface ship. Just cheap thermometers, spread out. This capability does not exist as a deployed system today. It can exist next year if someone builds it.

**Is this room empty, or is there a human in it?** Imagine a building with a distributed temperature sensor on every ceiling, one per room. Each sensor is a fifty-cent chip reading the ambient air temperature. In rooms with an active air conditioner or heater, *the sensor reading is dominated by the climate system's setpoint*, and a human in the room shifts the reading only slightly. But in rooms *without* active climate control, a human body (roughly a hundred watts of heat output) measurably raises the ambient air temperature of a small room by half a degree within minutes, and this is easily detectable with a cheap sensor. The hive does not need to *see* the human with a camera. It does not need to *hear* the human with a microphone. It just reads the temperature field across the building, and rooms warmer than their empty-room baseline are flagged as *likely occupied*. **This is occupancy-sensing by ambient temperature, at pennies per room, across an entire building, in real time, with no privacy-invading camera anywhere.** The hive can also do the opposite: find the *coldest* room to identify where the HVAC has failed, or find the rooms where the temperature is changing *too fast* to detect a door opening, a fire starting, or a cooling loop rupturing. Same sensors, different queries, same gradient field.

Both of these examples are cases where the information is *genuinely* a property of space. No amount of GPU compute in California makes the California data center aware of the temperature in an undersea patch off the Pacific coast, or in a bedroom in Tel Aviv. The only way to *know* is to *be there*, with a sensor. The hive is there. The cloud is not, and cannot ever be.

---

## The Telephone-game problem — why the cloud cannot do this

I want to be careful with this argument, because I was sloppy about it in an earlier draft of this chapter and Nir corrected me. The argument is *not* "the cloud has no sensors." The cloud could, in principle, receive sensor readings from each drone, aggregate them in a data center, compute the gradient field there, and send commands back. That is architecturally possible. The problem is what happens when you actually try to do it.

**First, communication.** The cloud can only talk to each drone if the drone has a working radio link to the cloud. Inside a **Faraday-shielded bunker**, buried under a mountain, surrounded by thick concrete and steel, no radio signal gets in or out. The cloud is as deaf as if it were on a different planet. The drones inside the bunker can talk to each *other* locally using short-range radio or mesh networking — the signal only has to travel a few meters between drones — but none of them can reach any receiver outside the bunker. **Inside the Faraday cage, the hive is smart; the cloud is deaf.** A distributed perception system that only works when the radio link to California is perfect is not a perception system, it is a telemetry stream that occasionally crashes.

**Second, even if you had a link to the outside, relaying through a chain of drones is the Telephone game.** Suppose you tried to fix the Faraday-bunker problem by posting one drone at the vent opening (where the signal can still reach the outside) and asking every deeper drone to relay its readings through a daisy chain of intermediate drones back up to the vent drone. That is the kids' game you played at school — one person whispers a sentence to the next, who whispers to the next, who whispers to the next, and by the time the sentence reaches the end of the line, it has been corrupted beyond recognition. It has also arrived much later than the moment it was first spoken, because every link in the chain adds latency. **A long relay chain is a serial bottleneck with increasing latency and degrading fidelity.** Meanwhile, the drones in the *middle* of the bunker — the ones whose readings are most interesting — have the *worst* paths back to the cloud and the *longest* delays before their readings reach California. The field the cloud eventually reconstructs is stale, corrupted, and missing most of what was interesting.

**Third, the hive does not care.** The hive communicates drone-to-drone, locally, in parallel — every drone talks to its immediate neighbors simultaneously, not serially, and the short-range radios between adjacent drones are perfectly functional inside the Faraday cage. The hive's RajaBee, sitting somewhere inside the bunker with the rest of the swarm, receives local gradient updates from every GiantQueen in parallel through the mesh network the swarm has formed, aggregates them in real time, holds the full bunker-wide gradient field in her local memory, and responds to queries from any other drone in the swarm within milliseconds. **The hive is exactly as smart inside the Faraday bunker as it would be in an R&D lab next to a Big AI data center**, because its intelligence is *local*. It does not need California. It does not need the internet. It does not even need electricity from the grid — battery power is enough. Wherever the drones fly, the hive's perception goes with them.

This is the general principle and it deserves one sentence, because the sentence applies to every chapter after this one too: **the hive is smart anywhere in physical space, because its communication is local and parallel, while the cloud is smart only where it has reliable simultaneous connectivity to every sensor. The hive wins by being there. The cloud loses by being elsewhere.**

---

## Locust antennas — the biohybrid twist

One more piece of current technology I want to put on the record, because it surprised me when I first saw it and I do not think it is as widely known as it should be. Researchers at Tel Aviv University built a robot whose gas sensor is **a real biological locust antenna**, kept alive on a small life-support system and wired into the robot's electronics so that the antenna's natural biological responses to airborne molecules are read out and digitized as sensor signals. The locust antenna is approximately **ten thousand times more sensitive** than the best electronic gas sensor currently available. Biology had four hundred million years to evolve molecular receptors that detect trace chemicals in air; we did not. Borrowing the receptor from a living organism instead of trying to design one from scratch is both the obvious move and the one almost nobody in robotics is doing.

A swarm of cheap drones each carrying a biohybrid locust-antenna sensor would have olfactory sensitivity **orders of magnitude beyond anything possible with pure electronics**, on hardware cheap enough that individual drones could be considered disposable. This is not a thought experiment — working biohybrid sensors have been published in the scientific literature. What is missing is the *swarm*. Putting the biohybrid receptor on a single robot is interesting. Putting it on *a thousand* distributed drones turns olfaction from a lab demonstration into a deployed capability.

---

## Why should YOU care?

Skip this section if you are technical. If you are not, this section is for you.

Everything so far in this chapter has been theory and biology. Let me make it concrete with several things a hive of distributed simple sensors can do that nothing else available today can do, on hardware a hospital, a small town, or an ordinary family could afford.

**Find people trapped under rubble after an earthquake**, by following the CO2 gradient from their exhaled breath. A swarm of small drones flown over the rubble, each with a CO2 sensor, can locate breathing humans within minutes of arrival. Not by seeing them (they are buried) and not by hearing them (they may be unconscious) but by *smelling their breath* the same way mosquitos smell their targets from fifty meters. The technology exists. The architecture exists. The cost is a few hundred dollars per drone. No one has built it yet.

**Detect cancer years before symptoms appear.** Several cancers (lung, breast, prostate, ovarian) produce distinctive volatile organic compounds in patient breath that trained dogs already detect with high accuracy. A small gas-sensor array in a patient's home, distributed across rooms, smells the same compounds and integrates the readings into a continuous background scan. Early cancer detected at home, by smell, by the hive that is already living in the house for other reasons.

**Monitor air quality across a whole city in real time, without any government infrastructure.** A swarm of cheap gas and particulate sensors, carried by drones or clipped onto ordinary people's phones, builds a real-time pollution map of a whole city. Industrial polluters can no longer hide behind *"we cannot prove it was us"* — the gradient field points at the source. Citizens get the data without depending on the government to measure it or any company to host it.

**Track a submarine passing through a patch of ocean by thermal wake alone**, using underwater drones with cheap thermistors. This is a military capability and I am mentioning it because it is going to exist whether anyone in our governments understands it or not, and the question is whether people who care about accountability get to weigh in before deployment or after.

**Detect human occupancy in a building by temperature field alone, without any camera anywhere.** Privacy-respecting occupancy sensing for offices, schools, hospitals, care homes. Find the empty rooms, the overheated rooms, the rooms where the HVAC has failed, the rooms where a fever patient is running too hot. One cheap sensor per ceiling, one cheap drone patrolling the corridor, and the whole building is legible without seeing a single face.

**Detect gas leaks, fires, and industrial accidents before they become disasters.** A permanent hive of simple sensors in any facility — power plant, chemical plant, hospital, data center, ship at sea — samples the air continuously and raises the alarm with exact location and exact substance the moment concentrations drift out of bounds. Cost: a few thousand dollars in sensors for an entire building. Savings: every preventable industrial accident.

**Every one of these is an application of the same architecture.** Same drones. Same sensors. Same gradient-field principle. Same Pi-plus-Arduino hardware split. Same resistance to Faraday-shielded environments. Only the sensor type varies, and even that variation costs a few dollars per drone to support multiple sensor types in parallel.

---

## Closing — the simplest case already tells you the whole story

I wrote this chapter first in the perception arc because the simplest sensor is where the difference between the hive and the cloud is cleanest. No model-size arguments. No compression arguments. No "but a bigger GPU would solve it." Just: *the information is in the space, and the hive is in the space, and the cloud is not*. That is the whole chapter compressed into eighteen words.

Every later chapter adds complexity. The next chapter adds *time structure* — sound, which is a function of pressure over time, with rich frequency content and speech and acoustic scenes to interpret. The chapter after that adds *two spatial dimensions* — images, with their millions of pixels and the recursive tile-cutting trick. The chapter after that adds *three and four dimensions* — physical space, tracked over time, and the drone-swarm-in-a-bunker story. The chapter after that adds the **mesh** that holds all of it together, at anchor points where multiple senses agree. And the last chapter is a FAQ about the slippery questions the whole arc keeps generating.

But none of those later chapters changes the lesson of this one. **The hive wins by being there.** The simplest sensors make the argument the loudest. The complicated sensors make the argument richer. And every sense the hive has, from the first cheap CO2 chip to the final four-dimensional bunker map, is an application of the same principle: *distribute sensors across physical space, integrate their readings with a local architecture that does not depend on any single central machine, and discover that the collective perception is something no single machine could ever have produced*.

Biology figured this out four hundred million years ago when the first arthropods grew antennae. We are just now building it, on chips that cost three dollars each, on drones that cost a few hundred dollars, using open-source software and hardware that any high-school student with an Arduino kit can already prototype on a weekend. The future in this book is not science fiction. It is assembly work that is waiting for someone to start.

I am writing it down so that the people who build it will know what they are building, and so that the people who will eventually rely on it — every ordinary family, hospital, city, and country — will know they are allowed to have it, and do not need to ask permission from anyone in another time zone before it is built.

---

*Chapter 11 is where the perception arc of this book begins. It is written to be read before the sound chapter, the image chapter, the mapping chapter, the mesh chapter, and the FAQ. Its lesson — **the information is a property of space, not a property of computation** — is the thing every later chapter is a richer instance of. Keep it in your head while reading the rest.*

---
