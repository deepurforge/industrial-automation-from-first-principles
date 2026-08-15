# Industrial Processes and Systems

> Before learning sensors, PLCs, control logic, or networks — understand the physical process those technologies are trying to control.

Every chapter before this one answered a question about automation itself: what it is, where it came from, why it exists. This chapter steps back and asks something more basic, and easy to skip past without ever really answering:

> **What exactly is happening inside a factory before automation enters the picture at all?**

If you can't answer that clearly, every sensor, controller, and network you learn about later will feel like abstract machinery with nothing underneath it. This chapter is the ground floor.

---

## Table of Contents

1. [What Is an Industrial Process?](#1-what-is-an-industrial-process)
2. [Process vs. Machine vs. System](#2-process-vs-machine-vs-system)
3. [Every Industrial Process Has a Purpose](#3-every-industrial-process-has-a-purpose)
4. [The Universal Structure of an Industrial Process](#4-the-universal-structure-of-an-industrial-process)
5. [Inputs — What Enters the Process?](#5-inputs--what-enters-the-process)
6. [Outputs — What Leaves the Process?](#6-outputs--what-leaves-the-process)
7. [Physical Variables](#7-physical-variables)
8. [The State of a Process](#8-the-state-of-a-process)
9. [Controlled Variable, Manipulated Variable, Disturbance](#9-controlled-variable-manipulated-variable-disturbance)
10. [Disturbances, in Depth](#10-disturbances-in-depth)
11. [Measurement — A Window Into the Process](#11-measurement--a-window-into-the-process)
12. [Actuation — How the System Changes Reality](#12-actuation--how-the-system-changes-reality)
13. [Open-Loop Control](#13-open-loop-control)
14. [Closed-Loop Control](#14-closed-loop-control)
15. [Types of Industrial Processes](#15-types-of-industrial-processes)
16. [Unit Operation → Machine → Line → Plant](#16-unit-operation--machine--line--plant)
17. [Systems Are Made of Interacting Subsystems](#17-systems-are-made-of-interacting-subsystems)
18. [System Boundaries](#18-system-boundaries)
19. [Interaction Between Systems](#19-interaction-between-systems)
20. [Complete Example — The Water Tank System](#20-complete-example--the-water-tank-system)
21. [The Complete First-Principles Model](#21-the-complete-first-principles-model)
22. [What Have We Actually Learned?](#22-what-have-we-actually-learned)
23. [References](#23-references)

---

## 1. What Is an Industrial Process?

Strip away every sensor, screen, and controller, and a factory is still doing exactly one thing: **taking something in one physical state and turning it into something in a different, more useful state.**

![Raw material entering a steel mill](https://upload.wikimedia.org/wikipedia/commons/d/d1/Steel_mill_in_Lorain.jpg)
*Iron ore and coke go in; steel comes out. No PLC, sensor, or network changes that basic transformation — they only make it faster, safer, and more consistent. (Source: [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Steel_mill_in_Lorain.jpg))*

![Raw material through physical process to product](assets/diagrams/01-process-chain.png)

That's it. That's an industrial process, in its most stripped-down form — ore becomes steel, flour becomes bread, crude oil becomes fuel, raw water becomes drinking water. Automation doesn't replace this transformation. It sits *around* it, watching and steering it. Before you can understand the watching and steering, you have to be able to see the transformation clearly on its own — which is the entire point of this chapter.

---

## 2. Process vs. Machine vs. System

These three words get used interchangeably in casual conversation, and that habit quietly causes confusion in every later chapter of this repository. Pull them apart now, once, carefully.

| Term | Definition | Example |
|---|---|---|
| **Process** | The physical transformation or operation taking place | Heating water |
| **Machine** | The equipment performing the operation | An industrial heater |
| **System** | The collection of interacting components that together achieve a purpose | Tank + heater + sensor + valve + controller + pump + pipes |

![System decomposed into machine, sensors, and control, with machine performing the process](assets/diagrams/02-process-machine-system.png)

Here's why this distinction earns its own section instead of a one-line footnote: **a machine can exist without a system, and a process can exist without either.** A hand-cranked water pump performs a *process* (moving water) using a *machine* (the pump) with no *system* around it at all — no sensor, no controller, nothing measuring or deciding anything. The moment you add a level sensor and a controller that opens or closes a valve based on what that sensor reads, you now have a *system*. Keep these three words distinct, and terms like "process variable," "process control," and "control system" — which get used constantly starting in Chapter 05 — will make immediate sense instead of blurring together.

---

## 3. Every Industrial Process Has a Purpose

Before any of the variables, sensors, or control logic covered later matter at all, ask the simplest possible question: **what is this process actually trying to accomplish?**

Nearly every industrial process reduces to one or more of these fundamental actions:

- Heat something
- Cool something
- Move something
- Mix materials
- Separate materials
- Fill containers
- Press or form material
- Cut material
- Transport material
- Generate electricity
- Maintain a pressure, level, or temperature at a target

![Industrial process purposes: heat, move, mix, separate, form, each with an example machine](assets/diagrams/03-process-purpose-map.png)

This matters because **the purpose of a process determines everything downstream of it** — which variables need watching, what kind of sensor is appropriate, what "success" even means. You cannot pick the right sensor for a process whose purpose you haven't first named clearly.

---

## 4. The Universal Structure of an Industrial Process

Almost every industrial process, regardless of industry, reduces to one shape.

**The simple version:**
```
INPUT → PROCESS → OUTPUT
```

**The real version** — the one that everything else in this chapter, and this repository, builds on:

![The universal process structure: input and control action into the process, disturbances acting on it, output leaving it](assets/diagrams/04-universal-structure.png)

Five ideas live inside that one diagram, and each gets its own section below:

- **Inputs** — material, energy, or information entering the process.
- **Outputs** — product, waste, or information leaving it.
- **Disturbances** — unplanned changes that affect the process from outside anyone's control.
- **Control actions** — deliberate adjustments made to keep the process on target.
- **Measurements** — the information that makes control actions possible in the first place.

This single diagram is the foundation for everything in control engineering. Every PLC program, every control loop, every SCADA screen you'll encounter later in this repository is, underneath, an implementation of this shape.

---

## 5. Inputs — What Enters the Process?

Inputs are not only electrical signals — that's a common beginner assumption, and it's worth correcting immediately. Inputs fall into three genuinely different categories:

![Inputs split into material, energy, and information](assets/diagrams/05-inputs-tree.png)

| Category | Examples |
|---|---|
| **Material** | Water, raw material, fuel, chemicals, parts |
| **Energy** | Electrical, mechanical, thermal, compressed air, hydraulic |
| **Information** | Production command, setpoint, recipe, operator command |

Notice that "information" belongs on this list at all — a setpoint entered by an operator is just as much an input to the process as the raw water flowing into a tank. Later chapters on control logic are really about that third category: how information inputs get turned into decisions.

---

## 6. Outputs — What Leaves the Process?

Symmetrically, a process can produce:

- **Product** — the intended result.
- **Waste** — unintended but unavoidable byproduct.
- **Heat, motion, pressure, or flow** — physical outputs, sometimes intended, sometimes not.
- **Electrical energy** — the direct output of a power generation process.
- **Information** — a measurement, a status, a completion signal.

> An output can be physical or informational — and a well-designed system treats the informational outputs (measurements, statuses, alarms) as just as essential as the physical ones. A process that makes a perfect product but reports nothing about itself is invisible to everyone trying to manage it.

---

## 7. Physical Variables

This is where the chapter starts pointing directly at what automation engineers actually spend their time observing and manipulating.

![Physical process variables grouped into thermal, mechanical, and fluid categories](assets/diagrams/07-physical-variable-map.png)

| Category | Core Variables |
|---|---|
| Thermal | Temperature |
| Mechanical | Speed, position, force, torque, weight, vibration |
| Fluid | Pressure, flow, level |
| Electrical | Voltage, current |
| Chemical | pH |

Every one of these variables is something that must eventually be turned into an electrical signal a controller can read — which is exactly the subject of Chapter 05. For now, the point is simpler: **learn to see a process in terms of these variables.** A tank isn't just "a tank" — it's a level, a temperature, a pressure, and a flow rate, all existing simultaneously, all capable of being measured and controlled independently.

---

## 8. The State of a Process

Here's a genuinely deep, first-principles idea hiding inside something that sounds obvious: **a process has a state** — a complete snapshot of every relevant variable at one moment in time.

Take a tank, at one specific instant:

| Variable | Value |
|---|---|
| Level | 70% |
| Temperature | 65 °C |
| Pressure | 2 bar |
| Flow | 20 L/min |

Together, these four numbers describe part of the tank's current state. A moment later, if the outlet valve opens, the level starts falling and a new state exists — same tank, same equipment, different state. This is a subtle but important shift in thinking: **automation doesn't control a "tank." It controls a state, continuously, as that state tries to drift away from where it should be.**

![Level changes, sensor value changes, process state changes — a simple causal chain](assets/diagrams/08-state-change-chain.png)

---

## 9. Controlled Variable, Manipulated Variable, Disturbance

Three terms that will appear constantly starting in the control-theory chapters — worth locking in precisely now, with one consistent example.

| Term | Definition | Tank Example |
|---|---|---|
| **Controlled Variable (CV)** | What you want to control | Tank level = 70% |
| **Manipulated Variable (MV)** | What the controller actually changes | Valve opening = 45% |
| **Disturbance** | Something that changes the process without being the intended control action | A sudden increase in outlet flow |

Notice the asymmetry: the controller never touches the controlled variable directly. It can't reach into the tank and move the water level by hand. It can only adjust the manipulated variable — the valve — and *hope*, based on a correct understanding of the process, that adjusting the valve produces the desired change in level. That gap between "what I want to control" and "what I'm actually allowed to touch" is the central engineering problem of every control loop you'll study later.

---

## 10. Disturbances, in Depth

A disturbance deserves its own section because it's the concept most beginner explanations skip past — and it's the actual reason feedback control exists at all.

![Disturbance loop: demand rises, tank level falls, controller adjusts the valve to correct it](assets/diagrams/10-disturbance-loop.png)

> A good control system doesn't merely produce an output. It responds when the real world changes unexpectedly.

Walk through the sequence a disturbance actually triggers:

1. The tank is operating normally, level steady at setpoint.
2. Outlet demand suddenly increases — more water is being drawn out.
3. Level begins decreasing.
4. The sensor detects the change.
5. The controller compares the new reading against the setpoint and calculates a correction.
6. The valve adjusts — opening further to compensate.
7. Level returns toward target.

Notice that *nobody told the controller* the demand was going to increase. It didn't need to be told. It only needed to notice the effect and correct for it. That is the entire value proposition of feedback control in one sequence.

---

## 11. Measurement — A Window Into the Process

Here's a transition worth sitting with, because it explains why instrumentation gets an entire chapter of its own right after this one:

> The controller cannot directly "see" the physical world. It needs measurements.

![Measurement chain: physical variable through sensor, electrical signal, input system, to controller](assets/diagrams/11-measurement-chain.png)

A PLC has no eyes, no hands, no sense of temperature. Everything it "knows" about the physical world arrives exclusively through this chain — a physical quantity, converted by a sensor into an electrical signal, read by an input system, and only then available to the controller as a number it can compare and act on. If that chain breaks anywhere — a failed sensor, a damaged cable, a miscalibrated signal — the controller isn't just "confused." It's making decisions based on a physical world that no longer matches reality. This is exactly why Chapter 05 exists: measurement isn't a minor supporting detail. It's the entire basis of everything a control system does.

---

## 12. Actuation — How the System Changes Reality

Now reverse the path. Measurement is how the system *perceives* reality; actuation is how it *changes* reality.

![Actuation chain: controller through output signal, actuator, physical action, to process changes](assets/diagrams/12a-actuation-chain.png)

Common actuators: motors, valves, heaters, cylinders, solenoids, drives.

![CNC machine — a controller's decisions becoming physical motion](https://upload.wikimedia.org/wikipedia/commons/f/f0/CNC_Mill_1.jpg)
*Every actuator is the same idea at different scales: an electrical decision becoming a physical action. (Source: [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:CNC_Mill_1.jpg), CC BY-SA 4.0)*

Put measurement and actuation side by side, and the complete information loop of every automated system becomes visible:

![Measurement and action forming one continuous loop between physical world and controller](assets/diagrams/12b-measure-action-loop.png)

This is quietly one of the most important diagrams in this entire repository. Every chapter from here forward is really just adding detail to one side of this loop or the other — more detail on measurement (Chapter 05), or more detail on actuation and control logic (Chapters 06–07).

---

## 13. Open-Loop Control

The simplest possible way to control anything — and the version that fails as soon as reality gets unpredictable.

![Open-loop control: command to controller to actuator to process, with no feedback path](assets/diagrams/13-open-loop.png)

Notice what's missing: no arrow leads back from the process to the controller. **No feedback.**

> Example: a heater is switched ON for 10 minutes, regardless of the actual temperature reached.

This works perfectly — right up until conditions change from whatever they were when the timing was calculated. A colder room, a different starting temperature, a partially open window — none of it gets noticed, because nothing is measuring the actual result. Open-loop control isn't a "wrong" way to do things; it's genuinely the correct, simplest choice for plenty of real tasks. But it only works when the relationship between command and outcome is reliable and unaffected by disturbances — which, as Section 10 already showed, is rarely true in a real industrial environment.

---

## 14. Closed-Loop Control

This is the concept Chapter 03 already introduced from the "why" angle. Here it is again, precisely, from the "what" angle.

![Closed-loop control: setpoint, controller, actuator, process, and sensor feedback closing the loop](assets/diagrams/14-closed-loop.png)

The difference from open-loop control is exactly one arrow — the path from process, through a sensor, back to the controller. That single arrow is the entire concept of feedback, and it changes everything: the controller now knows what actually happened, not just what it commanded to happen.

Walk the sequence: setpoint is set → process changes → sensor measures the result → controller compares that result to the setpoint → controller reacts → process is corrected → sensor measures again. This loop never stops running. It is, without exaggeration, the single most important structural idea in all of industrial automation — every PID loop, every PLC control block, and every advanced control strategy covered later in this repository is a variation on this exact diagram.

---

## 15. Types of Industrial Processes

Not every process runs the same way over time. Four broad categories cover almost everything you'll encounter.

### A. Continuous processes

Material flows in and product flows out, essentially without interruption.

```
Material →→→ Process →→→ Product
```

Examples: oil refining, chemical plants, water treatment, power generation.

### B. Batch processes

A defined quantity is processed as a discrete "batch," start to finish, before the next one begins.

![Batch process: load, process, hold, unload](assets/diagrams/15a-batch-process.png)

![Industrial planetary mixer — a batch process in progress](https://upload.wikimedia.org/wikipedia/commons/4/40/Industrial_planetary_mixer.jpg)
*Mixing 1,000 L of a chemical to a fixed recipe — load, process, hold, unload, then start the next batch. (Source: [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Industrial_planetary_mixer.jpg))*

Example: mixing a fixed recipe to a target volume, then discharging it before starting the next batch.

### C. Discrete processes

Individual, countable units move through distinct stages.

![Discrete process: part, assembly, inspection, packaging](assets/diagrams/15b-discrete-process.png)

Example: an assembly line, where each unit is a separate, trackable item.

### D. Hybrid processes

Most real plants combine all three — a continuous upstream process feeding a batch mixing stage, followed by discrete packaging.

| Type | Example |
|---|---|
| Continuous | Refinery, water treatment plant |
| Batch | Chemical mixing to a recipe |
| Discrete | Assembly line, packaging line |
| Hybrid | Most real-world plants |

Recognizing which category a process falls into isn't academic — it determines the entire control philosophy applied to it. A continuous process is usually held at a steady operating point; a batch process is guided through a sequence of distinct phases; a discrete process is tracked unit by unit. The control strategies covered later in this repository diverge sharply along exactly this line.

---

## 16. Unit Operation → Machine → Line → Plant

Zoom out, one level at a time, and a single physical phenomenon scales all the way up to an enterprise.

![Scaling hierarchy from physical phenomenon up through unit operation, machine, production cell, line, plant, to enterprise](assets/diagrams/16-scaling-hierarchy.png)

A **unit operation** is the smallest meaningful physical step — heating, mixing, filtering. A **machine** physically performs one or more unit operations. A **production cell** groups machines that work together on one task. A **production line** strings cells together into a sequence. A **plant** houses multiple lines. An **enterprise** may operate multiple plants. Every level in that stack is built from the level below it — which means a fault at the unit-operation level (one clogged filter) can, left unaddressed, eventually show up as a missed shipment at the enterprise level. Understanding this hierarchy is what lets an engineer trace a problem from a boardroom report all the way down to a single stuck valve.

---

## 17. Systems Are Made of Interacting Subsystems

Zoom into any single automated system, and it decomposes further into subsystems that each do one job.

![Pumping system decomposed into measurement, control, and actuation subsystems, each with their components](assets/diagrams/17-pumping-subsystems.png)

> Failure or change in one subsystem can affect the entire system.

A pressure sensor drifting out of calibration doesn't just produce a wrong number on a screen — it can cause the controller to make a wrong decision, which moves an actuator incorrectly, which changes the actual physical process. Nothing about the pump or the motor needs to fail for the whole system to misbehave. This is the exact mindset needed for troubleshooting, covered in later chapters: a symptom in one subsystem often has its root cause in a completely different one.

---

## 18. System Boundaries

A question most beginner material skips entirely, and one worth asking deliberately: **where does a system begin, and where does it end?**

![System boundary drawn around tank, pump, valve, sensors, and controller — water in, process output out](assets/diagrams/18-system-boundary.png)

The boundary is a choice, not a physical fact — and the same physical object can sit in different places depending on which system you're currently analyzing:

- A pump can be **inside** the "pumping system" boundary.
- The same pump is **outside** the boundary of the "tank level control system," if that system is defined more narrowly.
- The same pump can be a **subsystem** of a larger "plant water system."

This isn't a pedantic distinction — it's essential for real engineering analysis. Troubleshooting, documentation, and responsibility all depend on everyone agreeing where one system's boundary ends and the next one's begins.

---

## 19. Interaction Between Systems

Systems rarely exist in isolation. They connect to each other through defined interfaces.

![Three systems connected through interfaces: A to B to C](assets/diagrams/19a-system-interaction-generic.png)

A concrete industrial version of that same chain:

![Industrial interaction chain: machine to PLC to industrial network to SCADA to MES](assets/diagrams/19b-system-interaction-industrial.png)

Each arrow in that diagram is an interface — a defined, agreed-upon way for one system to hand information (or material, or energy) to the next. This is the exact bridge into the industrial architecture and communication chapters later in this repository: none of those technologies exist in a vacuum. They exist specifically to move information cleanly across interfaces like these.

---

## 20. Complete Example — The Water Tank System

Every concept in this chapter, tied together in one deliberately simple case study.

![Water treatment tanks — the same tank-level principle, at industrial scale](https://upload.wikimedia.org/wikipedia/commons/5/5a/Waste_Water_Treatment_Plant.jpg)
*Real water treatment infrastructure — underneath the scale and complexity, the same tank-and-valve loop below is doing the core work. (Source: [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Waste_Water_Treatment_Plant.jpg), CC BY-SA 4.0)*

![Complete water tank system: water in, valve, tank, pump, water out, with level sensor and PLC closing the control loop back to the valve](assets/diagrams/20-tank-system-complete.png)

Now map every term from this entire chapter onto this one system:

| Element | Role |
|---|---|
| Water | Process material |
| Tank | Process |
| Level | Controlled variable |
| Valve | Manipulated element |
| Sensor | Measurement |
| PLC | Controller |
| Outlet demand | Disturbance |
| Water out | Process output |

If every row of that table makes sense to you without re-reading an earlier section, this chapter has done its job. This single tank is, structurally, no different from a distillation column, a reactor, or a furnace — different physics, identical logical shape.

---

## 21. The Complete First-Principles Model

Everything in this chapter compresses into one diagram.

![Master first-principles model: real world through physical process, disturbances and output, sensor, information, controller, control action, and actuator, closing the loop back to the process](assets/diagrams/21-master-model.png)

> Industrial automation is built around the interaction between a physical process, measurements, decisions, and physical actions.

That sentence is the entire chapter, said once, plainly. Everything above it exists to make sure that sentence actually means something to you, instead of just sounding correct.

---

## 22. What Have We Actually Learned?

You should now be able to answer, without hesitation:

- What is a process? What is a system? What's the difference between a machine and a process?
- What are inputs and outputs — and why does "information" count as both?
- What are physical variables, and how are they grouped?
- What is the *state* of a process?
- What is a controlled variable? A manipulated variable? A disturbance?
- Why is measurement necessary — what can't a controller do without it?
- How does an actuator turn a decision into a physical change?
- What is open-loop control, and when does it fail?
- What is closed-loop control, and why does that one feedback arrow matter so much?
- What separates continuous, batch, and discrete processes?
- What is a subsystem? A system boundary? How do systems interact through interfaces?

That list is not a quiz — it's the exact toolkit the next chapter assumes you already have.

```
Sensors → Measurement → Control → Actuation → PLCs
```

Chapter 05 picks up exactly where Section 11 of this chapter left off: **how do you actually turn a physical variable into a signal a controller can trust?**

---

## 23. References

- ISA (International Society of Automation) — process control terminology and standards.
- IEC 61512 / ISA-88 — batch control standards (referenced conceptually).
- Perry's Chemical Engineers' Handbook — unit operations framework.
- Wikimedia Commons — photographs credited individually above each image.

---

## Suggested Repository Structure for This Chapter

```
industrial-automation-from-first-principles/
│
├── README.md
│
├── 01-fundamentals/
│   ├── 01-what-is-industrial-automation.md
│   ├── 02-where-did-industrial-automation-begin-history.md
│   ├── 03-why-industrial-automation-exists.md
│   └── 04-industrial-processes-and-systems.md
│
└── assets/
    ├── images/
    ├── diagrams/
    ├── charts/
    └── animations/
```

**Notes for building this out on GitHub:**
- Every diagram in this chapter is a **generated PNG image** in `assets/diagrams/`, not Mermaid — upload that folder alongside this file so the images resolve correctly on GitHub.
- Photographs are hot-linked to Wikimedia Commons with credit lines intact; download into `assets/images/` for a fully offline repository.
- **Section 20 (the water tank system) and Section 14 (closed-loop control) are the strongest candidates in this chapter for a companion animated `.html` file**, following the same pattern as Chapters 02 and 03 — happy to build that next.
- Keep a source/reference line under every image and diagram, as done throughout this file.
