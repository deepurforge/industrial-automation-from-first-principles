# 05 — Measurement and Instrumentation Fundamentals

> **Measure the physical world. Understand the signal. Trust the information.**

---

## 0. Why This Chapter Exists

Automation cannot control what it cannot measure. Every control decision a PLC makes — opening a valve, starting a motor, tripping an alarm — is only as good as the measurement that fed it. This chapter builds the bridge between **physical reality** and the **digital information** a control system acts on.

A physical quantity is not automatically "data." It becomes trustworthy information only after it passes through a chain of sensing, conversion, conditioning, transmission, and interpretation — and at every step, something can go wrong. Understanding that chain, and where it can fail, is the real subject of this chapter.

By the end of this chapter you should be able to answer:

- What does it actually mean to *measure* something?
- How does a physical phenomenon become an electrical signal, and then a number in a PLC?
- Why do industrial signals look the way they do (4–20 mA, 0–10 V, HART, discrete)?
- How do you judge whether a measurement can be trusted?
- How do you trace a measurement problem back to its root cause?

![Physical process instrumented with sensors feeding a control system](data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAxNDAwIDQyMCIgZm9udC1mYW1pbHk9IlNlZ29lIFVJLCBBcmlhbCwgc2Fucy1zZXJpZiI+CiAgPHJlY3QgeD0iMCIgeT0iMCIgd2lkdGg9IjE0MDAiIGhlaWdodD0iNDIwIiBmaWxsPSIjMGYxNzJhIi8+CiAgPHRleHQgeD0iNzAwIiB5PSI0NSIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZmlsbD0iI2UyZThmMCIgZm9udC1zaXplPSIyNiIgZm9udC13ZWlnaHQ9ImJvbGQiPlBoeXNpY2FsIFByb2Nlc3Mg4oaSIFRydXN0ZWQgSW5mb3JtYXRpb248L3RleHQ+CgogIDwhLS0gQm94ZXMgLS0+CiAgPGcgZm9udC1zaXplPSIxNiIgZmlsbD0iIzBmMTcyYSI+CiAgICA8IS0tIFByb2Nlc3MgLS0+CiAgICA8cmVjdCB4PSIzMCIgeT0iMTQwIiB3aWR0aD0iMTUwIiBoZWlnaHQ9IjkwIiByeD0iMTAiIGZpbGw9IiMzOGJkZjgiLz4KICAgIDx0ZXh0IHg9IjEwNSIgeT0iMTgwIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXdlaWdodD0iYm9sZCI+UGh5c2ljYWw8L3RleHQ+CiAgICA8dGV4dCB4PSIxMDUiIHk9IjIwMCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC13ZWlnaHQ9ImJvbGQiPlByb2Nlc3M8L3RleHQ+CgogICAgPHJlY3QgeD0iMjIwIiB5PSIxNDAiIHdpZHRoPSIxNTAiIGhlaWdodD0iOTAiIHJ4PSIxMCIgZmlsbD0iIzRhZGU4MCIvPgogICAgPHRleHQgeD0iMjk1IiB5PSIxODAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtd2VpZ2h0PSJib2xkIj5TZW5zb3I8L3RleHQ+CgogICAgPHJlY3QgeD0iNDEwIiB5PSIxNDAiIHdpZHRoPSIxNzAiIGhlaWdodD0iOTAiIHJ4PSIxMCIgZmlsbD0iI2ZhY2MxNSIvPgogICAgPHRleHQgeD0iNDk1IiB5PSIxNzUiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtd2VpZ2h0PSJib2xkIj5TaWduYWw8L3RleHQ+CiAgICA8dGV4dCB4PSI0OTUiIHk9IjE5NSIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC13ZWlnaHQ9ImJvbGQiPkNvbmRpdGlvbmluZzwvdGV4dD4KCiAgICA8cmVjdCB4PSI2MjAiIHk9IjE0MCIgd2lkdGg9IjE3MCIgaGVpZ2h0PSI5MCIgcng9IjEwIiBmaWxsPSIjZmI5MjNjIi8+CiAgICA8dGV4dCB4PSI3MDUiIHk9IjE4MCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC13ZWlnaHQ9ImJvbGQiPlRyYW5zbWl0dGVyPC90ZXh0PgoKICAgIDxyZWN0IHg9IjgzMCIgeT0iMTQwIiB3aWR0aD0iMTUwIiBoZWlnaHQ9IjkwIiByeD0iMTAiIGZpbGw9IiNmNDcyYjYiLz4KICAgIDx0ZXh0IHg9IjkwNSIgeT0iMTc1IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXdlaWdodD0iYm9sZCI+UExDIEkvTzwvdGV4dD4KICAgIDx0ZXh0IHg9IjkwNSIgeT0iMTk1IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXdlaWdodD0iYm9sZCI+LyBEQVE8L3RleHQ+CgogICAgPHJlY3QgeD0iMTAyMCIgeT0iMTQwIiB3aWR0aD0iMTUwIiBoZWlnaHQ9IjkwIiByeD0iMTAiIGZpbGw9IiNhNzhiZmEiLz4KICAgIDx0ZXh0IHg9IjEwOTUiIHk9IjE4MCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC13ZWlnaHQ9ImJvbGQiPlBMQzwvdGV4dD4KCiAgICA8cmVjdCB4PSIxMjEwIiB5PSIxNDAiIHdpZHRoPSIxNjAiIGhlaWdodD0iOTAiIHJ4PSIxMCIgZmlsbD0iIzIyZDNlZSIvPgogICAgPHRleHQgeD0iMTI5MCIgeT0iMTc1IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXdlaWdodD0iYm9sZCI+SE1JIC88L3RleHQ+CiAgICA8dGV4dCB4PSIxMjkwIiB5PSIxOTUiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtd2VpZ2h0PSJib2xkIj5TQ0FEQTwvdGV4dD4KICA8L2c+CgogIDwhLS0gQXJyb3dzIC0tPgogIDxnIHN0cm9rZT0iI2UyZThmMCIgc3Ryb2tlLXdpZHRoPSI0IiBmaWxsPSJub25lIiBtYXJrZXItZW5kPSJ1cmwoI2Fycm93KSI+CiAgICA8bGluZSB4MT0iMTgwIiB5MT0iMTg1IiB4Mj0iMjE1IiB5Mj0iMTg1Ii8+CiAgICA8bGluZSB4MT0iMzcwIiB5MT0iMTg1IiB4Mj0iNDA1IiB5Mj0iMTg1Ii8+CiAgICA8bGluZSB4MT0iNTgwIiB5MT0iMTg1IiB4Mj0iNjE1IiB5Mj0iMTg1Ii8+CiAgICA8bGluZSB4MT0iNzkwIiB5MT0iMTg1IiB4Mj0iODI1IiB5Mj0iMTg1Ii8+CiAgICA8bGluZSB4MT0iOTgwIiB5MT0iMTg1IiB4Mj0iMTAxNSIgeTI9IjE4NSIvPgogICAgPGxpbmUgeDE9IjExNzAiIHkxPSIxODUiIHgyPSIxMjA1IiB5Mj0iMTg1Ii8+CiAgPC9nPgoKICA8ZGVmcz4KICAgIDxtYXJrZXIgaWQ9ImFycm93IiBtYXJrZXJXaWR0aD0iMTAiIG1hcmtlckhlaWdodD0iMTAiIHJlZlg9IjgiIHJlZlk9IjUiIG9yaWVudD0iYXV0byI+CiAgICAgIDxwYXRoIGQ9Ik0wLDAgTDEwLDUgTDAsMTAgWiIgZmlsbD0iI2UyZThmMCIvPgogICAgPC9tYXJrZXI+CiAgPC9kZWZzPgoKICA8dGV4dCB4PSI3MDAiIHk9IjMwMCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZmlsbD0iIzk0YTNiOCIgZm9udC1zaXplPSIxOCI+CiAgICBFdmVyeSBsaW5rIGluIHRoaXMgY2hhaW4gY2FuIGludHJvZHVjZSBlcnJvciDigJQgdHJ1c3R3b3J0aHkgaW5mb3JtYXRpb24gcmVxdWlyZXMgYWxsIG9mIHRoZW0gd29ya2luZyB0b2dldGhlci4KICA8L3RleHQ+CiAgPHRleHQgeD0iNzAwIiB5PSIzNDAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZpbGw9IiM2NDc0OGIiIGZvbnQtc2l6ZT0iMTUiIGZvbnQtc3R5bGU9Iml0YWxpYyI+CiAgICBGaWd1cmUgMSDigJQgVGhlIGNvbXBsZXRlIG1lYXN1cmVtZW50IGNoYWluIGZyb20gcGh5c2ljYWwgcmVhbGl0eSB0byBvcGVyYXRvciBkZWNpc2lvbi4KICA8L3RleHQ+Cjwvc3ZnPgo=)
*Figure 1 — From physical process to decision: sensor → signal conditioning → transmitter → PLC I/O → controller → HMI/SCADA.*

This chapter connects directly to [04 — Industrial Processes and Systems](../04-industrial-processes-and-systems.md) (the *what* being measured) and sets up [06 — Actuators and Final Control Elements](../06-actuators-and-final-control-elements.md) and the upcoming PLC chapters (the *what happens next*).

---

## 1. What Is Measurement?

Measurement is not simply "reading a sensor." It is the process of comparing a physical quantity against a reference or standard to produce a **value with an associated uncertainty**. NIST defines measurement as a process that produces an estimate of a quantity together with the uncertainty of that estimate — the number alone is not the whole story.

Key vocabulary:

| Term | Meaning |
|---|---|
| **Measurand** | The specific quantity intended to be measured (e.g., "temperature of the fluid at the tank outlet") |
| **Reference / standard** | A known, traceable quantity used for comparison |
| **Measurement result** | The value obtained from the measurement process |
| **Measurement uncertainty** | The doubt that exists about the result — how far it might be from the true value |
| **Indication** | What the instrument displays or outputs — not necessarily the true physical quantity |

```
Physical phenomenon
        ↓
     Measurand
        ↓
 Measurement process
        ↓
   Measured value
        ↓
    Uncertainty
        ↓
      Decision
```

The reader should internalize this before touching a single sensor: **every measurement is an estimate, not a fact.** Good instrumentation design is about keeping that estimate close enough to reality for the decision that depends on it.

---

## 2. Why Industrial Automation Needs Measurement

Ask the practical question: *why can't a PLC simply control a machine without measurement?*

```
No measurement
      ↓
No knowledge of process state
      ↓
No reliable feedback
      ↓
No meaningful control
```

**Manual process:**
`Human observes → Human decides → Human acts`

**Automated process:**

```mermaid
flowchart LR
    S[Sensor] --> M[Measurement]
    M --> C[Controller]
    C --> A[Actuator]
    A --> P[Process]
    P -. Feedback .-> S
```

Take away the measurement, and the feedback loop is broken — the controller is acting blind. This is the single most important idea in this chapter: **control quality has a hard ceiling set by measurement quality.** No control algorithm, however advanced, can compensate for a measurement that doesn't reflect reality.

---

## 3. What Exactly Are We Measuring?

Industrial systems measure a recurring set of physical quantities:

| Domain | Typical Units |
|---|---|
| Temperature | °C, °F, K |
| Pressure | Pa, bar, psi |
| Flow | L/min, m³/h |
| Level | mm, %, m |
| Position | mm, degrees |
| Speed | rpm, m/s |
| Force | N |
| Torque | N·m |
| Weight / Mass | kg |
| Electrical | V, A, Ω, W |
| Vibration | mm/s, acceleration (g) |
| Chemical | pH, conductivity, concentration |

![Factory scene with sensors on a tank, motor, pipe, conveyor, valve, and electrical panel](data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAxMjAwIDcwMCIgZm9udC1mYW1pbHk9IlNlZ29lIFVJLCBBcmlhbCwgc2Fucy1zZXJpZiI+CiAgPHJlY3QgeD0iMCIgeT0iMCIgd2lkdGg9IjEyMDAiIGhlaWdodD0iNzAwIiBmaWxsPSIjZjFmNWY5Ii8+CiAgPHRleHQgeD0iNjAwIiB5PSI0MCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIyNiIgZm9udC13ZWlnaHQ9ImJvbGQiIGZpbGw9IiMwZjE3MmEiPkluc3RydW1lbnRlZCBQcm9jZXNzIEFyZWE8L3RleHQ+CgogIDwhLS0gVGFuayAtLT4KICA8cmVjdCB4PSI4MCIgeT0iMTUwIiB3aWR0aD0iMTgwIiBoZWlnaHQ9IjI2MCIgcng9IjEwIiBmaWxsPSIjYmZkYmZlIiBzdHJva2U9IiMxZTQwYWYiIHN0cm9rZS13aWR0aD0iMyIvPgogIDx0ZXh0IHg9IjE3MCIgeT0iMTQwIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjE2IiBmb250LXdlaWdodD0iYm9sZCI+VGFuazwvdGV4dD4KICA8Y2lyY2xlIGN4PSIxNzAiIGN5PSIxNzAiIHI9IjEwIiBmaWxsPSIjZGMyNjI2Ii8+CiAgPHRleHQgeD0iMTkwIiB5PSIxNzUiIGZvbnQtc2l6ZT0iMTMiPkxldmVsIChyYWRhcik8L3RleHQ+CiAgPGNpcmNsZSBjeD0iMTcwIiBjeT0iMzgwIiByPSIxMCIgZmlsbD0iI2RjMjYyNiIvPgogIDx0ZXh0IHg9IjE5MCIgeT0iMzg1IiBmb250LXNpemU9IjEzIj5UZW1wZXJhdHVyZSAoUlREKTwvdGV4dD4KCiAgPCEtLSBQaXBlIC0tPgogIDxyZWN0IHg9IjI2MCIgeT0iMjMwIiB3aWR0aD0iMjIwIiBoZWlnaHQ9IjI0IiBmaWxsPSIjOTRhM2I4IiBzdHJva2U9IiMzMzQxNTUiIHN0cm9rZS13aWR0aD0iMiIvPgogIDx0ZXh0IHg9IjM3MCIgeT0iMjE1IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjEzIiBmb250LXdlaWdodD0iYm9sZCI+UGlwZTwvdGV4dD4KICA8Y2lyY2xlIGN4PSIzNzAiIGN5PSIyNDIiIHI9IjkiIGZpbGw9IiNkYzI2MjYiLz4KICA8dGV4dCB4PSIzOTAiIHk9IjI0NyIgZm9udC1zaXplPSIxMyI+RmxvdyAoQ29yaW9saXMpPC90ZXh0PgoKICA8IS0tIFZhbHZlIC0tPgogIDxwb2x5Z29uIHBvaW50cz0iNDgwLDIyMCA1MjAsMjQyIDQ4MCwyNjQgNDgwLDIyMCIgZmlsbD0iI2ZiYmYyNCIgc3Ryb2tlPSIjOTI0MDBlIiBzdHJva2Utd2lkdGg9IjIiLz4KICA8cG9seWdvbiBwb2ludHM9IjU2MCwyMjAgNTIwLDI0MiA1NjAsMjY0IDU2MCwyMjAiIGZpbGw9IiNmYmJmMjQiIHN0cm9rZT0iIzkyNDAwZSIgc3Ryb2tlLXdpZHRoPSIyIi8+CiAgPHRleHQgeD0iNTIwIiB5PSIyMDAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTMiIGZvbnQtd2VpZ2h0PSJib2xkIj5WYWx2ZTwvdGV4dD4KICA8Y2lyY2xlIGN4PSI1MjAiIGN5PSIyNDIiIHI9IjkiIGZpbGw9IiNkYzI2MjYiLz4KICA8dGV4dCB4PSI1NDAiIHk9IjI5MCIgZm9udC1zaXplPSIxMyI+UG9zaXRpb24gZmVlZGJhY2s8L3RleHQ+CgogIDwhLS0gUGlwZSBjb250aW51ZXMgLS0+CiAgPHJlY3QgeD0iNTYwIiB5PSIyMzAiIHdpZHRoPSIxNDAiIGhlaWdodD0iMjQiIGZpbGw9IiM5NGEzYjgiIHN0cm9rZT0iIzMzNDE1NSIgc3Ryb2tlLXdpZHRoPSIyIi8+CgogIDwhLS0gTW90b3IgLS0+CiAgPHJlY3QgeD0iNzAwIiB5PSIxODAiIHdpZHRoPSIxMjAiIGhlaWdodD0iMTIwIiByeD0iMTIiIGZpbGw9IiM4NmVmYWMiIHN0cm9rZT0iIzE2NjUzNCIgc3Ryb2tlLXdpZHRoPSIzIi8+CiAgPGNpcmNsZSBjeD0iNzYwIiBjeT0iMjQwIiByPSIzNSIgZmlsbD0iIzRhZGU4MCIgc3Ryb2tlPSIjMTY2NTM0IiBzdHJva2Utd2lkdGg9IjIiLz4KICA8dGV4dCB4PSI3NjAiIHk9IjE2MCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMyIgZm9udC13ZWlnaHQ9ImJvbGQiPk1vdG9yPC90ZXh0PgogIDxjaXJjbGUgY3g9IjgyMCIgY3k9IjI0MCIgcj0iOSIgZmlsbD0iI2RjMjYyNiIvPgogIDx0ZXh0IHg9IjgzNiIgeT0iMjMwIiBmb250LXNpemU9IjEzIj5TcGVlZDwvdGV4dD4KICA8dGV4dCB4PSI4MzYiIHk9IjI0OCIgZm9udC1zaXplPSIxMyI+VmlicmF0aW9uPC90ZXh0PgoKICA8IS0tIENvbnZleW9yIC0tPgogIDxyZWN0IHg9IjgwIiB5PSI0NzAiIHdpZHRoPSI1MDAiIGhlaWdodD0iMzAiIHJ4PSIxNSIgZmlsbD0iI2NiZDVlMSIgc3Ryb2tlPSIjNDc1NTY5IiBzdHJva2Utd2lkdGg9IjIiLz4KICA8Y2lyY2xlIGN4PSIxMTAiIGN5PSI0ODUiIHI9IjE2IiBmaWxsPSIjNDc1NTY5Ii8+CiAgPGNpcmNsZSBjeD0iNTUwIiBjeT0iNDg1IiByPSIxNiIgZmlsbD0iIzQ3NTU2OSIvPgogIDx0ZXh0IHg9IjMzMCIgeT0iNDU1IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjEzIiBmb250LXdlaWdodD0iYm9sZCI+Q29udmV5b3I8L3RleHQ+CiAgPGNpcmNsZSBjeD0iMzMwIiBjeT0iNDcwIiByPSI5IiBmaWxsPSIjZGMyNjI2Ii8+CiAgPHRleHQgeD0iMzUwIiB5PSI0NTAiIGZvbnQtc2l6ZT0iMTMiPlByb3hpbWl0eSAvIHNwZWVkPC90ZXh0PgoKICA8IS0tIEVsZWN0cmljYWwgcGFuZWwgLS0+CiAgPHJlY3QgeD0iOTAwIiB5PSI0MjAiIHdpZHRoPSIxODAiIGhlaWdodD0iMjIwIiByeD0iOCIgZmlsbD0iI2UyZThmMCIgc3Ryb2tlPSIjMzM0MTU1IiBzdHJva2Utd2lkdGg9IjMiLz4KICA8dGV4dCB4PSI5OTAiIHk9IjQwNSIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMyIgZm9udC13ZWlnaHQ9ImJvbGQiPkVsZWN0cmljYWwgUGFuZWw8L3RleHQ+CiAgPHJlY3QgeD0iOTIwIiB5PSI0NDAiIHdpZHRoPSIxNDAiIGhlaWdodD0iMjAiIGZpbGw9IiNmOGZhZmMiIHN0cm9rZT0iIzk0YTNiOCIvPgogIDxyZWN0IHg9IjkyMCIgeT0iNDcwIiB3aWR0aD0iMTQwIiBoZWlnaHQ9IjIwIiBmaWxsPSIjZjhmYWZjIiBzdHJva2U9IiM5NGEzYjgiLz4KICA8cmVjdCB4PSI5MjAiIHk9IjUwMCIgd2lkdGg9IjE0MCIgaGVpZ2h0PSIyMCIgZmlsbD0iI2Y4ZmFmYyIgc3Ryb2tlPSIjOTRhM2I4Ii8+CiAgPGNpcmNsZSBjeD0iMTA4MCIgY3k9IjQ1MCIgcj0iOSIgZmlsbD0iI2RjMjYyNiIvPgogIDx0ZXh0IHg9IjEwMDAiIHk9IjU2MCIgZm9udC1zaXplPSIxMyI+ViAvIEEgLyBrVyBtZXRlcmluZzwvdGV4dD4KCiAgPHRleHQgeD0iNjAwIiB5PSI2NzAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZpbGw9IiM0NzU1NjkiIGZvbnQtc2l6ZT0iMTUiIGZvbnQtc3R5bGU9Iml0YWxpYyI+CiAgICBGaWd1cmUgMiDigJQgQSB0eXBpY2FsIHByb2Nlc3MgYXJlYSByYXJlbHkgcmVsaWVzIG9uIGEgc2luZ2xlIG1lYXN1cmVtZW50IHR5cGUuCiAgPC90ZXh0Pgo8L3N2Zz4K)
*Figure 2 — A typical process area rarely relies on a single measurement type; temperature, pressure, level, and flow instruments usually coexist on the same skid.*

---

## 4. The Measurement System — The Complete Chain

This is the backbone diagram of the entire chapter. Every instrumentation topic that follows fits somewhere on this chain.

```mermaid
flowchart LR
    A[Physical Process] --> B[Measurand]
    B --> C[Sensor]
    C --> D[Transducer]
    D --> E[Signal Conditioning]
    E --> F[Transmitter / Transmission]
    F --> G[PLC I/O / DAQ]
    G --> H[Controller]
    H --> I[HMI / SCADA]
    I --> J[Decision / Action]
```

Keep this chain in mind as a mental checklist: whenever a number on an HMI looks wrong, it is wrong at *one specific link* in this chain — and finding which link is the core diagnostic skill of instrumentation work (see §22, "Diagnostic Thinking").

---

## 5. Sensor vs. Transducer vs. Transmitter vs. Instrument

These four words get used loosely in industry, but they mean distinct things:

- **Sensor** — detects and responds to a physical quantity (e.g., a thermocouple junction).
- **Transducer** — converts one form of energy or quantity into another usable form (often used interchangeably with "sensor," but strictly the *conversion* element).
- **Transmitter** — converts the measurement into a standardized signal suitable for transmission over distance (e.g., 4–20 mA, HART).
- **Instrument** — the broader device or system that indicates, records, or processes measurement information; may bundle sensor + transmitter in one housing.

```
Physical quantity
      ↓
   SENSOR
      ↓
 TRANSDUCER
      ↓
    SIGNAL
      ↓
 TRANSMITTER
      ↓
STANDARDIZED SIGNAL
      ↓
INSTRUMENT / PLC
```

> **Note:** real industrial products blur these lines constantly — a "pressure transmitter" is a sensor, transducer, and transmitter in a single package. The terminology matters for understanding *function*, not for memorizing rigid product categories.

---

## 6. From Physics to Electricity

This is where instrumentation becomes engineering. A physical change has to become an electrical change before a PLC can ever see it.

```
Temperature  → Resistance change   → Electrical signal
Pressure     → Diaphragm deflection → Electrical signal
Position     → Magnetic/optical/mechanical change → Electrical signal
```

![Diagram of mechanical, thermal, magnetic, optical, and chemical phenomena converging into an electrical signal](data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAxMTAwIDU2MCIgZm9udC1mYW1pbHk9IlNlZ29lIFVJLCBBcmlhbCwgc2Fucy1zZXJpZiI+CiAgPHJlY3QgeD0iMCIgeT0iMCIgd2lkdGg9IjExMDAiIGhlaWdodD0iNTYwIiBmaWxsPSIjZmFmYWY5Ii8+CiAgPHRleHQgeD0iNTUwIiB5PSI0MCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIyNCIgZm9udC13ZWlnaHQ9ImJvbGQiIGZpbGw9IiMxYzE5MTciPkV2ZXJ5IFNlbnNvciBJcyBhbiBFbmVyZ3ktRG9tYWluIFRyYW5zbGF0b3I8L3RleHQ+CgogIDxnIGZvbnQtc2l6ZT0iMTUiIGZvbnQtd2VpZ2h0PSJib2xkIiBmaWxsPSIjMWMxOTE3Ij4KICAgIDxyZWN0IHg9IjQwIiB5PSI5MCIgd2lkdGg9IjIwMCIgaGVpZ2h0PSI2MCIgcng9IjEwIiBmaWxsPSIjZmVjYWNhIi8+CiAgICA8dGV4dCB4PSIxNDAiIHk9IjEyNSIgdGV4dC1hbmNob3I9Im1pZGRsZSI+TWVjaGFuaWNhbDwvdGV4dD4KCiAgICA8cmVjdCB4PSI0MCIgeT0iMTcwIiB3aWR0aD0iMjAwIiBoZWlnaHQ9IjYwIiByeD0iMTAiIGZpbGw9IiNmZWQ3YWEiLz4KICAgIDx0ZXh0IHg9IjE0MCIgeT0iMjA1IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj5UaGVybWFsPC90ZXh0PgoKICAgIDxyZWN0IHg9IjQwIiB5PSIyNTAiIHdpZHRoPSIyMDAiIGhlaWdodD0iNjAiIHJ4PSIxMCIgZmlsbD0iI2ZlZjA4YSIvPgogICAgPHRleHQgeD0iMTQwIiB5PSIyODUiIHRleHQtYW5jaG9yPSJtaWRkbGUiPk1hZ25ldGljPC90ZXh0PgoKICAgIDxyZWN0IHg9IjQwIiB5PSIzMzAiIHdpZHRoPSIyMDAiIGhlaWdodD0iNjAiIHJ4PSIxMCIgZmlsbD0iI2JiZjdkMCIvPgogICAgPHRleHQgeD0iMTQwIiB5PSIzNjUiIHRleHQtYW5jaG9yPSJtaWRkbGUiPk9wdGljYWw8L3RleHQ+CgogICAgPHJlY3QgeD0iNDAiIHk9IjQxMCIgd2lkdGg9IjIwMCIgaGVpZ2h0PSI2MCIgcng9IjEwIiBmaWxsPSIjYmZkYmZlIi8+CiAgICA8dGV4dCB4PSIxNDAiIHk9IjQ0NSIgdGV4dC1hbmNob3I9Im1pZGRsZSI+Q2hlbWljYWw8L3RleHQ+CiAgPC9nPgoKICA8IS0tIENvbnZlcmdpbmcgbGluZXMgLS0+CiAgPGcgc3Ryb2tlPSIjNzg3MTZjIiBzdHJva2Utd2lkdGg9IjMiIGZpbGw9Im5vbmUiPgogICAgPGxpbmUgeDE9IjI0MCIgeTE9IjEyMCIgeDI9IjQ4MCIgeTI9IjI4MCIvPgogICAgPGxpbmUgeDE9IjI0MCIgeTE9IjIwMCIgeDI9IjQ4MCIgeTI9IjI4MCIvPgogICAgPGxpbmUgeDE9IjI0MCIgeTE9IjI4MCIgeDI9IjQ4MCIgeTI9IjI4MCIvPgogICAgPGxpbmUgeDE9IjI0MCIgeTE9IjM2MCIgeDI9IjQ4MCIgeTI9IjI4MCIvPgogICAgPGxpbmUgeDE9IjI0MCIgeTE9IjQ0MCIgeDI9IjQ4MCIgeTI9IjI4MCIvPgogIDwvZz4KCiAgPGNpcmNsZSBjeD0iNTIwIiBjeT0iMjgwIiByPSI2MCIgZmlsbD0iI2Y4ZmFmYyIgc3Ryb2tlPSIjMWMxOTE3IiBzdHJva2Utd2lkdGg9IjMiLz4KICA8dGV4dCB4PSI1MjAiIHk9IjI3NSIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxNSIgZm9udC13ZWlnaHQ9ImJvbGQiPlNlbnNpbmc8L3RleHQ+CiAgPHRleHQgeD0iNTIwIiB5PSIyOTUiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTUiIGZvbnQtd2VpZ2h0PSJib2xkIj5FbGVtZW50PC90ZXh0PgoKICA8bGluZSB4MT0iNTgwIiB5MT0iMjgwIiB4Mj0iNzYwIiB5Mj0iMjgwIiBzdHJva2U9IiMxYzE5MTciIHN0cm9rZS13aWR0aD0iNCIgbWFya2VyLWVuZD0idXJsKCNhcnJvdzIpIi8+CiAgPGRlZnM+CiAgICA8bWFya2VyIGlkPSJhcnJvdzIiIG1hcmtlcldpZHRoPSIxMCIgbWFya2VySGVpZ2h0PSIxMCIgcmVmWD0iOCIgcmVmWT0iNSIgb3JpZW50PSJhdXRvIj4KICAgICAgPHBhdGggZD0iTTAsMCBMMTAsNSBMMCwxMCBaIiBmaWxsPSIjMWMxOTE3Ii8+CiAgICA8L21hcmtlcj4KICA8L2RlZnM+CgogIDxyZWN0IHg9Ijc4MCIgeT0iMjMwIiB3aWR0aD0iMjYwIiBoZWlnaHQ9IjEwMCIgcng9IjEwIiBmaWxsPSIjMGYxNzJhIi8+CiAgPHRleHQgeD0iOTEwIiB5PSIyNzAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTciIGZvbnQtd2VpZ2h0PSJib2xkIiBmaWxsPSIjZTJlOGYwIj5FbGVjdHJpY2FsPC90ZXh0PgogIDx0ZXh0IHg9IjkxMCIgeT0iMjk1IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjE3IiBmb250LXdlaWdodD0iYm9sZCIgZmlsbD0iI2UyZThmMCI+U2lnbmFsPC90ZXh0PgogIDx0ZXh0IHg9IjkxMCIgeT0iMzE1IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjEyIiBmaWxsPSIjOTRhM2I4Ij4oUiwgViwgSSwgQywgZik8L3RleHQ+CgogIDx0ZXh0IHg9IjU1MCIgeT0iNTIwIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmaWxsPSIjNTc1MzRlIiBmb250LXNpemU9IjE1IiBmb250LXN0eWxlPSJpdGFsaWMiPgogICAgRmlndXJlIDMg4oCUIE1lY2hhbmljYWwsIHRoZXJtYWwsIG1hZ25ldGljLCBvcHRpY2FsLCBhbmQgY2hlbWljYWwgcGhlbm9tZW5hIGFsbCBjb252ZXJnZSBpbnRvIG9uZSBjb21tb24gbGFuZ3VhZ2U6IGVsZWN0cmljaXR5LgogIDwvdGV4dD4KPC9zdmc+Cg==)
*Figure 3 — Every sensing technology is, at its core, a translator between an energy domain and electricity.*

---

## 7. Analog vs. Digital Measurement

**Analog** signals vary continuously: 4–20 mA, 0–10 V, thermocouple millivolts, RTD resistance.

**Digital** signals represent discrete states or discrete data: proximity sensor ON/OFF, limit switch contacts, encoder pulses, fieldbus communication.

```
Analog:                         Digital:
     ╭────╮                     ──────┐    ┌──────┐
─────╯    ╰────╮                      └────┘      └────
               ╰────
```

### 7.1 Discrete Measurement

Discrete instrumentation answers yes/no questions:

- Object detected? YES / NO
- Valve open? YES / NO
- Motor running? YES / NO
- Limit reached? YES / NO

Common technologies: proximity sensors, photoelectric sensors, limit switches, pressure switches, float switches — typically wired into a **24 V DC → PLC digital input**.

### 7.2 Continuous Measurement

Continuous instrumentation reports a value across a range:

```
Temperature = 73.4 °C
Pressure    = 5.72 bar
Flow        = 124.6 L/min
Level       = 67.3 %
```

Continuous measurement is what enables process control, alarming, trending, optimization, and closed-loop control — anywhere a controller needs to know *how much*, not just *yes or no*.

---

## 8. Common Industrial Sensor Technologies

Each subsection below follows the same discipline: the underlying physics first, then the practical engineering behavior that follows from it. Knowing *why* a sensor behaves the way it does is what lets you predict failure modes before you see them in the field.

### 8.1 Temperature

![RTD temperature probe used in industrial temperature measurement](data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA5MDAgNTAwIiBmb250LWZhbWlseT0iU2Vnb2UgVUksIEFyaWFsLCBzYW5zLXNlcmlmIj4KICA8cmVjdCB4PSIwIiB5PSIwIiB3aWR0aD0iOTAwIiBoZWlnaHQ9IjUwMCIgZmlsbD0iI2Y4ZmFmYyIvPgogIDx0ZXh0IHg9IjQ1MCIgeT0iNDAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMjQiIGZvbnQtd2VpZ2h0PSJib2xkIiBmaWxsPSIjMGYxNzJhIj5SVEQgKFB0MTAwKSBQcm9iZSDigJQgU3RydWN0dXJlICZhbXA7IEJlaGF2aW9yPC90ZXh0PgoKICA8IS0tIFByb2JlIGJvZHkgLS0+CiAgPHJlY3QgeD0iMTIwIiB5PSIxNTAiIHdpZHRoPSIyNjAiIGhlaWdodD0iMzQiIHJ4PSI2IiBmaWxsPSIjOTRhM2I4IiBzdHJva2U9IiMzMzQxNTUiIHN0cm9rZS13aWR0aD0iMiIvPgogIDx0ZXh0IHg9IjI1MCIgeT0iMTQwIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjEzIiBmb250LXdlaWdodD0iYm9sZCI+UHJvdGVjdGl2ZSBzaGVhdGg8L3RleHQ+CgogIDxyZWN0IHg9IjM4MCIgeT0iMTU4IiB3aWR0aD0iNjAiIGhlaWdodD0iMTgiIGZpbGw9IiNjYmQ1ZTEiIHN0cm9rZT0iIzMzNDE1NSIgc3Ryb2tlLXdpZHRoPSIyIi8+CgogIDwhLS0gc2Vuc2luZyB0aXAgLS0+CiAgPGNpcmNsZSBjeD0iMTQwIiBjeT0iMTY3IiByPSIxNyIgZmlsbD0iI2Y5NzMxNiIgc3Ryb2tlPSIjN2MyZDEyIiBzdHJva2Utd2lkdGg9IjIiLz4KICA8dGV4dCB4PSIxNDAiIHk9IjEyMCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMiIgZm9udC13ZWlnaHQ9ImJvbGQiPlB0IHNlbnNpbmcgZWxlbWVudDwvdGV4dD4KCiAgPCEtLSBjb25uZWN0aW9uIGhlYWQgLS0+CiAgPHJlY3QgeD0iNDQwIiB5PSIxMjAiIHdpZHRoPSIxMjAiIGhlaWdodD0iOTQiIHJ4PSIxMCIgZmlsbD0iI2UyZThmMCIgc3Ryb2tlPSIjMzM0MTU1IiBzdHJva2Utd2lkdGg9IjIiLz4KICA8dGV4dCB4PSI1MDAiIHk9IjExNSIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMiIgZm9udC13ZWlnaHQ9ImJvbGQiPkNvbm5lY3Rpb24gaGVhZDwvdGV4dD4KICA8bGluZSB4MT0iNTYwIiB5MT0iMTQwIiB4Mj0iNjQwIiB5Mj0iMTQwIiBzdHJva2U9IiMxZDRlZDgiIHN0cm9rZS13aWR0aD0iMyIvPgogIDxsaW5lIHgxPSI1NjAiIHkxPSIxNjciIHgyPSI2NDAiIHkyPSIxNjciIHN0cm9rZT0iI2RjMjYyNiIgc3Ryb2tlLXdpZHRoPSIzIi8+CiAgPGxpbmUgeDE9IjU2MCIgeTE9IjE5NCIgeDI9IjY0MCIgeTI9IjE5NCIgc3Ryb2tlPSIjMWQ0ZWQ4IiBzdHJva2Utd2lkdGg9IjMiLz4KICA8dGV4dCB4PSI2NjAiIHk9IjE0NSIgZm9udC1zaXplPSIxMiI+V2lyZSAxPC90ZXh0PgogIDx0ZXh0IHg9IjY2MCIgeT0iMTcyIiBmb250LXNpemU9IjEyIj5XaXJlIDI8L3RleHQ+CiAgPHRleHQgeD0iNjYwIiB5PSIxOTkiIGZvbnQtc2l6ZT0iMTIiPldpcmUgMyAoMy13aXJlKTwvdGV4dD4KCiAgPCEtLSBncmFwaCAtLT4KICA8ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSg4MCwyNjApIj4KICAgIDxsaW5lIHgxPSIwIiB5MT0iMTgwIiB4Mj0iNjAwIiB5Mj0iMTgwIiBzdHJva2U9IiMzMzQxNTUiIHN0cm9rZS13aWR0aD0iMiIvPgogICAgPGxpbmUgeDE9IjAiIHkxPSIxODAiIHgyPSIwIiB5Mj0iMCIgc3Ryb2tlPSIjMzM0MTU1IiBzdHJva2Utd2lkdGg9IjIiLz4KICAgIDx0ZXh0IHg9IjMwMCIgeT0iMjE1IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjE0IiBmb250LXdlaWdodD0iYm9sZCI+VGVtcGVyYXR1cmUgKMKwQyk8L3RleHQ+CiAgICA8dGV4dCB4PSItNDAiIHk9IjkwIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjE0IiBmb250LXdlaWdodD0iYm9sZCIgdHJhbnNmb3JtPSJyb3RhdGUoLTkwLC00MCw5MCkiPlJlc2lzdGFuY2UgKM6pKTwvdGV4dD4KICAgIDxwYXRoIGQ9Ik0wLDE3MCBMNTgwLDIwIiBzdHJva2U9IiNmOTczMTYiIHN0cm9rZS13aWR0aD0iNCIgZmlsbD0ibm9uZSIvPgogICAgPHRleHQgeD0iNDgwIiB5PSI0MCIgZm9udC1zaXplPSIxMyIgZmlsbD0iIzdjMmQxMiIgZm9udC13ZWlnaHQ9ImJvbGQiPlIoVCkgPSBS4oKAKDEgKyDOsVQpPC90ZXh0PgogICAgPHRleHQgeD0iMjAiIHk9IjIwMCIgZm9udC1zaXplPSIxMiI+MTAwIM6pIEAgMMKwQzwvdGV4dD4KICA8L2c+CgogIDx0ZXh0IHg9IjQ1MCIgeT0iNDkwIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmaWxsPSIjNDc1NTY5IiBmb250LXNpemU9IjE1IiBmb250LXN0eWxlPSJpdGFsaWMiPgogICAgRmlndXJlIDQg4oCUIEFuIFJURCdzIHJlc2lzdGFuY2UgcmlzZXMgcHJlZGljdGFibHkgYW5kIG5lYXItbGluZWFybHkgd2l0aCB0ZW1wZXJhdHVyZS4KICA8L3RleHQ+Cjwvc3ZnPgo=)
*Figure 4 — An RTD probe: resistance increases predictably with temperature, giving one of the most stable and repeatable temperature measurements available.*

**RTD (Resistance Temperature Detector).** A pure metal (almost always platinum) has a resistance that increases in a predictable, highly repeatable way as temperature rises. Over a limited industrial range this is approximated by a linear relationship:

```
R(T) = R₀ · (1 + α·T)
```

Where `R₀` is the resistance at 0 °C (100 Ω for a Pt100, 1000 Ω for a Pt1000), and `α` is the temperature coefficient of resistance (≈ 0.00385 Ω/Ω/°C for standard Pt100 per IEC 60751). Over wider ranges, the more precise **Callendar–Van Dusen equation** is used, which adds quadratic (and, below 0 °C, cubic) correction terms — this is why quality temperature transmitters store polynomial coefficients rather than a single linear slope.

RTDs are *passive* — they require an excitation current to produce a measurable voltage drop, and that same current causes **self-heating error** if too large, which is why RTD excitation currents are deliberately kept small (typically ~1 mA or less).

**Thermocouple.** Two dissimilar metal wires joined at a "hot junction" generate a small voltage (the **Seebeck effect**) proportional to the temperature difference between that junction and a "cold" (reference) junction:

```
V = S · (T_hot − T_cold)
```

Where `S` is the Seebeck coefficient of the metal pair (e.g., ~41 µV/°C for Type K near room temperature — and non-linear over wide ranges, which is why transmitters apply a lookup table or polynomial, not a single constant). Because the output depends on a *difference*, thermocouples require **cold-junction compensation** — the transmitter measures its own terminal temperature (often with a small onboard sensor) and adds back the voltage that junction would itself produce, to recover the true hot-junction temperature. Skipping this step is one of the most common thermocouple wiring mistakes in the field.

**Thermistor.** A semiconductor whose resistance changes sharply — and non-linearly — with temperature, typically decreasing as temperature rises (NTC — negative temperature coefficient). Far more sensitive than an RTD over a narrow span, but usable range is much smaller and the response is strongly non-linear, requiring more aggressive linearization.

**Temperature transmitter.** Regardless of which sensing element is used upstream, the transmitter's job is the same: apply the correct linearization curve, perform cold-junction compensation if needed, and output a standardized signal (4–20 mA, HART, or a fieldbus value) in engineering units.

| Technology | Typical Range | Accuracy Class | Notes |
|---|---|---|---|
| RTD (Pt100) | −200 °C to 850 °C | Very high, very stable | Needs excitation; self-heating risk |
| Thermocouple | −200 °C to 1800+ °C (type dependent) | Moderate | Widest range, rugged, needs cold-junction compensation |
| Thermistor | −50 °C to 150 °C (typical) | High sensitivity, narrow range | Strongly non-linear |

### 8.2 Pressure

![Pressure transmitter diaphragm sensing diagram](data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA5MDAgNTAwIiBmb250LWZhbWlseT0iU2Vnb2UgVUksIEFyaWFsLCBzYW5zLXNlcmlmIj4KICA8cmVjdCB4PSIwIiB5PSIwIiB3aWR0aD0iOTAwIiBoZWlnaHQ9IjUwMCIgZmlsbD0iI2Y4ZmFmYyIvPgogIDx0ZXh0IHg9IjQ1MCIgeT0iNDAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMjQiIGZvbnQtd2VpZ2h0PSJib2xkIiBmaWxsPSIjMGYxNzJhIj5QcmVzc3VyZSBUcmFuc21pdHRlciDigJQgRGlhcGhyYWdtIFNlbnNpbmc8L3RleHQ+CgogIDwhLS0gUHJvY2VzcyBwaXBlIC0tPgogIDxyZWN0IHg9IjYwIiB5PSIyMjAiIHdpZHRoPSIxNjAiIGhlaWdodD0iNTAiIGZpbGw9IiM5NGEzYjgiIHN0cm9rZT0iIzMzNDE1NSIgc3Ryb2tlLXdpZHRoPSIyIi8+CiAgPHRleHQgeD0iMTQwIiB5PSIyMTAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTMiIGZvbnQtd2VpZ2h0PSJib2xkIj5Qcm9jZXNzPC90ZXh0PgoKICA8IS0tIGltcHVsc2UgbGluZSAtLT4KICA8bGluZSB4MT0iMjIwIiB5MT0iMjQ1IiB4Mj0iMzIwIiB5Mj0iMjQ1IiBzdHJva2U9IiMzMzQxNTUiIHN0cm9rZS13aWR0aD0iNiIvPgoKICA8IS0tIERpYXBocmFnbSBob3VzaW5nIC0tPgogIDxyZWN0IHg9IjMyMCIgeT0iMTgwIiB3aWR0aD0iMTIwIiBoZWlnaHQ9IjEzMCIgcng9IjEwIiBmaWxsPSIjZTJlOGYwIiBzdHJva2U9IiMzMzQxNTUiIHN0cm9rZS13aWR0aD0iMyIvPgogIDxlbGxpcHNlIGN4PSIzODAiIGN5PSIyNDUiIHJ4PSIxMCIgcnk9IjQ1IiBmaWxsPSJub25lIiBzdHJva2U9IiNkYzI2MjYiIHN0cm9rZS13aWR0aD0iNCIvPgogIDx0ZXh0IHg9IjM4MCIgeT0iMzMwIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjEyIiBmb250LXdlaWdodD0iYm9sZCI+RGlhcGhyYWdtPC90ZXh0PgoKICA8IS0tIFNlbnNpbmcgZWxlbWVudCAtLT4KICA8cmVjdCB4PSI0NjAiIHk9IjIwNSIgd2lkdGg9IjExMCIgaGVpZ2h0PSI4MCIgcng9IjgiIGZpbGw9IiNmZWYwOGEiIHN0cm9rZT0iIzg1NGQwZSIgc3Ryb2tlLXdpZHRoPSIyIi8+CiAgPHRleHQgeD0iNTE1IiB5PSIyMzUiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTIiIGZvbnQtd2VpZ2h0PSJib2xkIj5QaWV6b3Jlc2lzdGl2ZTwvdGV4dD4KICA8dGV4dCB4PSI1MTUiIHk9IjI1MiIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMiIgZm9udC13ZWlnaHQ9ImJvbGQiPi8gQ2FwYWNpdGl2ZTwvdGV4dD4KICA8dGV4dCB4PSI1MTUiIHk9IjI2OSIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMiIgZm9udC13ZWlnaHQ9ImJvbGQiPmVsZW1lbnQ8L3RleHQ+CgogIDxsaW5lIHgxPSI0NDAiIHkxPSIyNDUiIHgyPSI0NjAiIHkyPSIyNDUiIHN0cm9rZT0iIzMzNDE1NSIgc3Ryb2tlLXdpZHRoPSIzIiBtYXJrZXItZW5kPSJ1cmwoI3BhKSIvPgoKICA8IS0tIEVsZWN0cm9uaWNzIC0tPgogIDxyZWN0IHg9IjYxMCIgeT0iMjAwIiB3aWR0aD0iMTUwIiBoZWlnaHQ9IjkwIiByeD0iOCIgZmlsbD0iI2JiZjdkMCIgc3Ryb2tlPSIjMTY2NTM0IiBzdHJva2Utd2lkdGg9IjIiLz4KICA8dGV4dCB4PSI2ODUiIHk9IjIzNSIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMiIgZm9udC13ZWlnaHQ9ImJvbGQiPkFtcGxpZnkgLzwvdGV4dD4KICA8dGV4dCB4PSI2ODUiIHk9IjI1NSIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMiIgZm9udC13ZWlnaHQ9ImJvbGQiPkxpbmVhcml6ZSAvPC90ZXh0PgogIDx0ZXh0IHg9IjY4NSIgeT0iMjc1IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjEyIiBmb250LXdlaWdodD0iYm9sZCI+Q29tcGVuc2F0ZTwvdGV4dD4KCiAgPGxpbmUgeDE9IjU3MCIgeTE9IjI0NSIgeDI9IjYwOCIgeTI9IjI0NSIgc3Ryb2tlPSIjMzM0MTU1IiBzdHJva2Utd2lkdGg9IjMiIG1hcmtlci1lbmQ9InVybCgjcGEpIi8+CgogIDxkZWZzPgogICAgPG1hcmtlciBpZD0icGEiIG1hcmtlcldpZHRoPSIxMCIgbWFya2VySGVpZ2h0PSIxMCIgcmVmWD0iOCIgcmVmWT0iNSIgb3JpZW50PSJhdXRvIj4KICAgICAgPHBhdGggZD0iTTAsMCBMMTAsNSBMMCwxMCBaIiBmaWxsPSIjMzM0MTU1Ii8+CiAgICA8L21hcmtlcj4KICA8L2RlZnM+CgogIDx0ZXh0IHg9IjY4NSIgeT0iMzMwIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjE0IiBmb250LXdlaWdodD0iYm9sZCIgZmlsbD0iIzE2NjUzNCI+T3V0cHV0OiA04oCTMjAgbUEgLyBIQVJUPC90ZXh0PgoKICA8dGV4dCB4PSI0NTAiIHk9IjQxMCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxNSIgZmlsbD0iIzMzNDE1NSI+R2F1Z2UgwrcgQWJzb2x1dGUgwrcgRGlmZmVyZW50aWFsIOKAlCBzYW1lIGNvcmUgc2Vuc2luZyBwcmluY2lwbGUsIGRpZmZlcmVudCByZWZlcmVuY2UgcG9pbnQ8L3RleHQ+CiAgPHRleHQgeD0iNDUwIiB5PSI0NjAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZpbGw9IiM0NzU1NjkiIGZvbnQtc2l6ZT0iMTUiIGZvbnQtc3R5bGU9Iml0YWxpYyI+CiAgICBGaWd1cmUgOCDigJQgUHJlc3N1cmUgZGVmbGVjdHMgYSBkaWFwaHJhZ207IHRoZSBzZW5zaW5nIGVsZW1lbnQgY29udmVydHMgdGhhdCBkZWZsZWN0aW9uIGludG8gYW4gZWxlY3RyaWNhbCBzaWduYWwuCiAgPC90ZXh0Pgo8L3N2Zz4K)
*Figure 5 — Pressure deflects a diaphragm; the sensing element converts that deflection into an electrical signal.*

```
Pressure
   ↓
Diaphragm deformation
   ↓
Sensing element (strain gauge / capacitive / piezoresistive)
   ↓
Electrical signal
   ↓
Transmitter
```

A thin diaphragm deflects under applied pressure; the sensing element converts that mechanical deflection into an electrical change. Two dominant approaches:

- **Piezoresistive / strain-gauge based** — deflection strains a resistive element bonded to the diaphragm; resistance change is measured via a Wheatstone bridge (see §8.7).
- **Capacitive** — the diaphragm forms one plate of a capacitor; deflection changes the gap (and therefore the capacitance) between it and a fixed reference plate.

Three pressure references matter, and confusing them is a common specification error:

| Type | Reference | Typical Notation |
|---|---|---|
| **Gauge pressure** | Local atmospheric pressure | barg, psig |
| **Absolute pressure** | Perfect vacuum | bara, psia |
| **Differential pressure** | Second process pressure | ΔP |

Differential pressure deserves special attention because it is used **indirectly** to infer both flow (via the orifice/Bernoulli relationship, see §8.4) and level (via hydrostatic pressure, see §8.3) — a single DP transmitter technology underlies three different "measurement types" on a P&ID.

### 8.3 Level

Multiple physical principles compete for the same job, each with different strengths:

- **Float** — mechanical, simple, robust, but has moving parts that can stick or foul.
- **Ultrasonic** — non-contact; a sound pulse is emitted downward and level is inferred from the round-trip time: `d = (v_sound × t) / 2`. Affected by vapor, temperature gradients (which change `v_sound`), and foam.
- **Radar** — non-contact; a microwave pulse is timed the same way ultrasonic is, but at the speed of light: `d = (c × t) / 2`. Handles vapor, dust, and temperature gradients far better than ultrasonic because electromagnetic wave speed barely changes with those conditions.
- **Differential pressure** — infers level from hydrostatic pressure at the bottom of a vessel: `P = ρ · g · h`, so `h = P / (ρ · g)`. Critically **depends on the fluid density `ρ`** — a DP level transmitter calibrated for water will read wrong if the fluid's density changes (e.g., temperature-driven density shift, or a different product in the tank).
- **Capacitive** — the probe and vessel wall form a capacitor; capacitance changes as the higher-dielectric-constant liquid replaces air along the probe.

![Radar level sensor mounted on a tank for continuous level measurement](data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA4MDAgNjAwIiBmb250LWZhbWlseT0iU2Vnb2UgVUksIEFyaWFsLCBzYW5zLXNlcmlmIj4KICA8cmVjdCB4PSIwIiB5PSIwIiB3aWR0aD0iODAwIiBoZWlnaHQ9IjYwMCIgZmlsbD0iI2YwZjlmZiIvPgogIDx0ZXh0IHg9IjQwMCIgeT0iNDAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMjQiIGZvbnQtd2VpZ2h0PSJib2xkIiBmaWxsPSIjMGM0YTZlIj5Ob24tQ29udGFjdCBSYWRhciBMZXZlbCBNZWFzdXJlbWVudDwvdGV4dD4KCiAgPCEtLSBUYW5rIC0tPgogIDxyZWN0IHg9IjIyMCIgeT0iMTIwIiB3aWR0aD0iMzYwIiBoZWlnaHQ9IjM4MCIgcng9IjgiIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzBjNGE2ZSIgc3Ryb2tlLXdpZHRoPSI0Ii8+CiAgPHJlY3QgeD0iMjI0IiB5PSIzMzAiIHdpZHRoPSIzNTIiIGhlaWdodD0iMTY2IiBmaWxsPSIjN2RkM2ZjIiBvcGFjaXR5PSIwLjciLz4KICA8dGV4dCB4PSI0MDAiIHk9IjMyMCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMyIgZmlsbD0iIzBjNGE2ZSI+VmFwb3Igc3BhY2U8L3RleHQ+CgogIDwhLS0gUmFkYXIgdHJhbnNtaXR0ZXIgLS0+CiAgPHJlY3QgeD0iMzYwIiB5PSI4MCIgd2lkdGg9IjgwIiBoZWlnaHQ9IjUwIiByeD0iOCIgZmlsbD0iI2Y5NzMxNiIgc3Ryb2tlPSIjN2MyZDEyIiBzdHJva2Utd2lkdGg9IjIiLz4KICA8dGV4dCB4PSI0MDAiIHk9IjcyIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjEzIiBmb250LXdlaWdodD0iYm9sZCI+UmFkYXIgVHJhbnNtaXR0ZXI8L3RleHQ+CiAgPHBvbHlnb24gcG9pbnRzPSIzODAsMTMwIDQyMCwxMzAgNDAwLDE1MCIgZmlsbD0iI2Y5NzMxNiIvPgoKICA8IS0tIHB1bHNlIC0tPgogIDxsaW5lIHgxPSI0MDAiIHkxPSIxNTAiIHgyPSI0MDAiIHkyPSIzMjUiIHN0cm9rZT0iI2RjMjYyNiIgc3Ryb2tlLXdpZHRoPSIzIiBzdHJva2UtZGFzaGFycmF5PSI4LDYiLz4KICA8dGV4dCB4PSI0MzAiIHk9IjI0MCIgZm9udC1zaXplPSIxMyIgZmlsbD0iI2RjMjYyNiIgZm9udC13ZWlnaHQ9ImJvbGQiPmQgPSBjIMK3IHQgLyAyPC90ZXh0PgogIDxsaW5lIHgxPSI0MDAiIHkxPSIzMjUiIHgyPSI0MDAiIHkyPSIxNTAiIHN0cm9rZT0iIzE2YTM0YSIgc3Ryb2tlLXdpZHRoPSIzIiBzdHJva2UtZGFzaGFycmF5PSI4LDYiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDIwLDApIi8+CgogIDx0ZXh0IHg9IjYyMCIgeT0iMjM1IiBmb250LXNpemU9IjEzIiBmaWxsPSIjMTZhMzRhIiBmb250LXdlaWdodD0iYm9sZCI+cmVmbGVjdGlvbjwvdGV4dD4KICA8dGV4dCB4PSI2MjAiIHk9IjI1NSIgZm9udC1zaXplPSIxMyIgZmlsbD0iI2RjMjYyNiIgZm9udC13ZWlnaHQ9ImJvbGQiPnB1bHNlIHNlbnQ8L3RleHQ+CgogIDwhLS0gbGlxdWlkIHN1cmZhY2UgLS0+CiAgPHBhdGggZD0iTTIyNCwzMzAgUTMwMCwzMjAgNDAwLDMzMCBUNTc2LDMzMCIgc3Ryb2tlPSIjMDI4NGM3IiBzdHJva2Utd2lkdGg9IjMiIGZpbGw9Im5vbmUiLz4KICA8dGV4dCB4PSI0MDAiIHk9IjQxNSIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxNSIgZm9udC13ZWlnaHQ9ImJvbGQiIGZpbGw9IiMwYzRhNmUiPkxpcXVpZDwvdGV4dD4KCiAgPCEtLSBsZXZlbCBpbmRpY2F0b3IgLS0+CiAgPGxpbmUgeDE9IjYwMCIgeTE9IjMzMCIgeDI9IjYwMCIgeTI9IjQ5NiIgc3Ryb2tlPSIjMzM0MTU1IiBzdHJva2Utd2lkdGg9IjIiLz4KICA8bGluZSB4MT0iNTkwIiB5MT0iMzMwIiB4Mj0iNjEwIiB5Mj0iMzMwIiBzdHJva2U9IiMzMzQxNTUiIHN0cm9rZS13aWR0aD0iMiIvPgogIDxsaW5lIHgxPSI1OTAiIHkxPSI0OTYiIHgyPSI2MTAiIHkyPSI0OTYiIHN0cm9rZT0iIzMzNDE1NSIgc3Ryb2tlLXdpZHRoPSIyIi8+CiAgPHRleHQgeD0iNjYwIiB5PSI0MjAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTMiPkxldmVsID0gaDwvdGV4dD4KCiAgPHRleHQgeD0iNDAwIiB5PSI1NjAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZpbGw9IiM0NzU1NjkiIGZvbnQtc2l6ZT0iMTUiIGZvbnQtc3R5bGU9Iml0YWxpYyI+CiAgICBGaWd1cmUgNSDigJQgQSBtaWNyb3dhdmUgcHVsc2UgaXMgdGltZWQgYXMgaXQgcmVmbGVjdHMgb2ZmIHRoZSBsaXF1aWQgc3VyZmFjZTsgcm91bmQtdHJpcCBkZWxheSBjb252ZXJ0cyBkaXJlY3RseSB0byBkaXN0YW5jZS4KICA8L3RleHQ+Cjwvc3ZnPgo=)
*Figure 6 — Non-contact radar level measurement: a microwave pulse is timed as it reflects off the liquid surface; the round-trip delay converts directly to distance, and distance converts to level once tank geometry is known.*

### 8.4 Flow

![Comparison of five flow measurement technologies](data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAxMTAwIDYwMCIgZm9udC1mYW1pbHk9IlNlZ29lIFVJLCBBcmlhbCwgc2Fucy1zZXJpZiI+CiAgPHJlY3QgeD0iMCIgeT0iMCIgd2lkdGg9IjExMDAiIGhlaWdodD0iNjAwIiBmaWxsPSIjZjhmYWZjIi8+CiAgPHRleHQgeD0iNTUwIiB5PSI0MCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIyNCIgZm9udC13ZWlnaHQ9ImJvbGQiIGZpbGw9IiMwZjE3MmEiPkZsb3cgTWVhc3VyZW1lbnQg4oCUIEZpdmUgVGVjaG5vbG9naWVzLCBGaXZlIFByaW5jaXBsZXM8L3RleHQ+CgogIDwhLS0gT3JpZmljZSAtLT4KICA8ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgzMCw4MCkiPgogICAgPHJlY3QgeD0iMCIgeT0iNDAiIHdpZHRoPSIyMDAiIGhlaWdodD0iMzAiIGZpbGw9IiM5NGEzYjgiLz4KICAgIDxwb2x5Z29uIHBvaW50cz0iOTAsNDAgMTEwLDQwIDEwNSw1NSA5NSw1NSIgZmlsbD0iI2RjMjYyNiIvPgogICAgPHRleHQgeD0iMTAwIiB5PSIyMCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMyIgZm9udC13ZWlnaHQ9ImJvbGQiPk9yaWZpY2UgKERQKTwvdGV4dD4KICAgIDx0ZXh0IHg9IjEwMCIgeT0iOTUiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTIiPlEg4oidIOKIms6UUDwvdGV4dD4KICA8L2c+CgogIDwhLS0gRWxlY3Ryb21hZ25ldGljIC0tPgogIDxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKDI2MCw4MCkiPgogICAgPHJlY3QgeD0iMCIgeT0iNDAiIHdpZHRoPSIyMDAiIGhlaWdodD0iMzAiIGZpbGw9IiM5NGEzYjgiLz4KICAgIDxjaXJjbGUgY3g9IjEwMCIgY3k9IjU1IiByPSIzNSIgZmlsbD0ibm9uZSIgc3Ryb2tlPSIjMjU2M2ViIiBzdHJva2Utd2lkdGg9IjMiIHN0cm9rZS1kYXNoYXJyYXk9IjQsNCIvPgogICAgPHRleHQgeD0iMTAwIiB5PSIyMCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMyIgZm9udC13ZWlnaHQ9ImJvbGQiPkVsZWN0cm9tYWduZXRpYzwvdGV4dD4KICAgIDx0ZXh0IHg9IjEwMCIgeT0iMTEwIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjEyIj5WIOKInSB2ZWxvY2l0eSAoQiBmaWVsZCk8L3RleHQ+CiAgPC9nPgoKICA8IS0tIFZvcnRleCAtLT4KICA8ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSg0OTAsODApIj4KICAgIDxyZWN0IHg9IjAiIHk9IjQwIiB3aWR0aD0iMjAwIiBoZWlnaHQ9IjMwIiBmaWxsPSIjOTRhM2I4Ii8+CiAgICA8cmVjdCB4PSI5NSIgeT0iMzUiIHdpZHRoPSIxMCIgaGVpZ2h0PSI0MCIgZmlsbD0iI2RjMjYyNiIvPgogICAgPHBhdGggZD0iTTEwNSw0MCBRMTMwLDQ1IDEwNSw1NSBRMTMwLDYwIDEwNSw3MCIgc3Ryb2tlPSIjMGVhNWU5IiBzdHJva2Utd2lkdGg9IjIiIGZpbGw9Im5vbmUiLz4KICAgIDx0ZXh0IHg9IjEwMCIgeT0iMjAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTMiIGZvbnQtd2VpZ2h0PSJib2xkIj5Wb3J0ZXggU2hlZGRpbmc8L3RleHQ+CiAgICA8dGV4dCB4PSIxMDAiIHk9IjExMCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMiI+ZnJlcSDiiJ0gdmVsb2NpdHk8L3RleHQ+CiAgPC9nPgoKICA8IS0tIENvcmlvbGlzIC0tPgogIDxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKDcyMCw4MCkiPgogICAgPHBhdGggZD0iTTAsNTUgUTUwLDIwIDEwMCw1NSBRMTUwLDkwIDIwMCw1NSIgc3Ryb2tlPSIjOTRhM2I4IiBzdHJva2Utd2lkdGg9IjE0IiBmaWxsPSJub25lIi8+CiAgICA8dGV4dCB4PSIxMDAiIHk9IjIwIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjEzIiBmb250LXdlaWdodD0iYm9sZCI+Q29yaW9saXM8L3RleHQ+CiAgICA8dGV4dCB4PSIxMDAiIHk9IjExNSIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMiI+cGhhc2Ugc2hpZnQg4oidIG1hc3MgZmxvdzwvdGV4dD4KICA8L2c+CgogIDwhLS0gVWx0cmFzb25pYyAtLT4KICA8ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSg5NTAsODApIj4KICAgIDxyZWN0IHg9IjAiIHk9IjQwIiB3aWR0aD0iMTIwIiBoZWlnaHQ9IjMwIiBmaWxsPSIjOTRhM2I4Ii8+CiAgICA8bGluZSB4MT0iMTAiIHkxPSIzMCIgeDI9IjExMCIgeTI9IjcwIiBzdHJva2U9IiNmOTczMTYiIHN0cm9rZS13aWR0aD0iMiIgc3Ryb2tlLWRhc2hhcnJheT0iMywzIi8+CiAgICA8dGV4dCB4PSI2MCIgeT0iMjAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTMiIGZvbnQtd2VpZ2h0PSJib2xkIj5VbHRyYXNvbmljPC90ZXh0PgogICAgPHRleHQgeD0iNjAiIHk9IjExMCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMiI+zpR0ICh3aXRoL2FnYWluc3QpPC90ZXh0PgogIDwvZz4KCiAgPCEtLSB0YWJsZSAtLT4KICA8ZyBmb250LXNpemU9IjE0Ij4KICAgIDx0ZXh0IHg9IjYwIiB5PSIyODAiIGZvbnQtd2VpZ2h0PSJib2xkIiBmaWxsPSIjMGYxNzJhIj5UZWNobm9sb2d5PC90ZXh0PgogICAgPHRleHQgeD0iMzMwIiB5PSIyODAiIGZvbnQtd2VpZ2h0PSJib2xkIiBmaWxsPSIjMGYxNzJhIj5CZXN0IFN1aXRlZCBGb3I8L3RleHQ+CiAgICA8dGV4dCB4PSI3MDAiIHk9IjI4MCIgZm9udC13ZWlnaHQ9ImJvbGQiIGZpbGw9IiMwZjE3MmEiPkxpbWl0YXRpb248L3RleHQ+CiAgICA8bGluZSB4MT0iMzAiIHkxPSIyOTUiIHgyPSIxMDcwIiB5Mj0iMjk1IiBzdHJva2U9IiNjYmQ1ZTEiLz4KCiAgICA8dGV4dCB4PSI2MCIgeT0iMzI1Ij5PcmlmaWNlIC8gRFA8L3RleHQ+CiAgICA8dGV4dCB4PSIzMzAiIHk9IjMyNSI+TG93LWNvc3QsIGdlbmVyYWwgc2VydmljZTwvdGV4dD4KICAgIDx0ZXh0IHg9IjcwMCIgeT0iMzI1Ij5TcXVhcmUtcm9vdCByZXNwb25zZSwgcG9vciB0dXJuZG93bjwvdGV4dD4KCiAgICA8dGV4dCB4PSI2MCIgeT0iMzYwIj5FbGVjdHJvbWFnbmV0aWM8L3RleHQ+CiAgICA8dGV4dCB4PSIzMzAiIHk9IjM2MCI+Q29uZHVjdGl2ZSBsaXF1aWRzLCBzbHVycmllczwvdGV4dD4KICAgIDx0ZXh0IHg9IjcwMCIgeT0iMzYwIj5GYWlscyBvbiBnYXNlcyAvIGh5ZHJvY2FyYm9uczwvdGV4dD4KCiAgICA8dGV4dCB4PSI2MCIgeT0iMzk1Ij5Wb3J0ZXg8L3RleHQ+CiAgICA8dGV4dCB4PSIzMzAiIHk9IjM5NSI+U3RlYW0sIGdhcywgY2xlYW4gbGlxdWlkczwvdGV4dD4KICAgIDx0ZXh0IHg9IjcwMCIgeT0iMzk1Ij5NaW5pbXVtIHZlbG9jaXR5IHJlcXVpcmVkPC90ZXh0PgoKICAgIDx0ZXh0IHg9IjYwIiB5PSI0MzAiPkNvcmlvbGlzPC90ZXh0PgogICAgPHRleHQgeD0iMzMwIiB5PSI0MzAiPkhpZ2gtYWNjdXJhY3kgbWFzcyBmbG93ICZhbXA7IGRlbnNpdHk8L3RleHQ+CiAgICA8dGV4dCB4PSI3MDAiIHk9IjQzMCI+SGlnaGVyIGNvc3QsIHByZXNzdXJlIGRyb3A8L3RleHQ+CgogICAgPHRleHQgeD0iNjAiIHk9IjQ2NSI+VWx0cmFzb25pYzwvdGV4dD4KICAgIDx0ZXh0IHg9IjMzMCIgeT0iNDY1Ij5Ob24taW52YXNpdmUgKGNsYW1wLW9uKSwgbGFyZ2UgcGlwZXM8L3RleHQ+CiAgICA8dGV4dCB4PSI3MDAiIHk9IjQ2NSI+U2Vuc2l0aXZlIHRvIHByb2ZpbGUgLyBidWJibGVzPC90ZXh0PgogIDwvZz4KCiAgPHRleHQgeD0iNTUwIiB5PSI1NTAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZpbGw9IiM0NzU1NjkiIGZvbnQtc2l6ZT0iMTUiIGZvbnQtc3R5bGU9Iml0YWxpYyI+CiAgICBGaWd1cmUgOSDigJQgRGlmZmVyZW50IHBoeXNpY2FsIHByaW5jaXBsZXMgYmVoYXZlIGRpZmZlcmVudGx5IHdpdGggdmlzY29zaXR5LCBjb25kdWN0aXZpdHksIGFuZCBwYXJ0aWN1bGF0ZXMuCiAgPC90ZXh0Pgo8L3N2Zz4K)
*Figure 7 — Different physical principles behave differently with viscosity, conductivity, and particulates.*

- **Differential pressure (orifice plate)** — a restriction in the pipe creates a measurable pressure drop related to flow rate by `Q ∝ √ΔP` (from the Bernoulli relationship) — note the **square-root relationship**, which means a DP flow transmitter has poor resolution at low flow and must often be signal-conditioned with a square-root extractor before scaling.
- **Electromagnetic** — applies Faraday's law of induction: a conductive fluid moving through a magnetic field induces a voltage proportional to velocity. Requires a conductive fluid; will not work on hydrocarbons, deionized water, or gases.
- **Vortex shedding** — a bluff body in the flow path sheds vortices at a frequency proportional to flow velocity (the Strouhal relationship); the meter counts shedding frequency rather than measuring a continuous analog value directly.
- **Coriolis** — the fluid is passed through a vibrating tube; the fluid's inertia causes a measurable phase shift (Coriolis effect) directly proportional to **mass** flow rate, not volumetric flow — this makes Coriolis meters uniquely able to report mass flow, density, and temperature from a single sensing element, at high accuracy, independent of fluid pressure/temperature compensation needed by other technologies.
- **Ultrasonic** — measures the transit-time difference of sound pulses sent with and against the flow direction; can be clamp-on (non-invasive) or inline (wetted, more accurate).

Each principle behaves differently with viscosity, conductivity, particulates, bubbles, and pipe orientation — the "right" flow meter is chosen from the fluid properties outward, not just from the flow range required.

### 8.5 Position and Proximity

| Technology | Target | Typical Use |
|---|---|---|
| Inductive | Metal only | Short-range detection, very rugged |
| Capacitive | Metal or non-metal | Detects most materials including liquids/granules |
| Photoelectric | Any (reflects/breaks light) | Longer range, sensitive to optical contamination |
| Magnetic (reed/Hall) | Magnet-equipped target | Cylinder position sensing |
| LVDT | Mechanical core, continuous | High-precision continuous linear position |
| Encoder (incremental/absolute) | Rotating shaft | Precise angular position/speed, feeds motion control |

An **incremental encoder** outputs pulses per revolution and requires a reference/home position to know absolute location; an **absolute encoder** outputs a unique code for every shaft position and knows its position immediately at power-up — an important distinction for machines that cannot tolerate a homing routine after every restart.

### 8.6 Speed

Speed measurement is fundamentally position measurement differentiated over time: pulses (from a proximity sensor or encoder) are counted over a known time window, or the time between pulses is measured directly (better for low speeds, where few pulses arrive per unit time). Tachometers and Hall-effect sensors follow the same underlying principle applied to rotating machinery.

### 8.7 Force and Torque

![Strain gauge Wheatstone bridge diagram](data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA5MDAgNTUwIiBmb250LWZhbWlseT0iU2Vnb2UgVUksIEFyaWFsLCBzYW5zLXNlcmlmIj4KICA8cmVjdCB4PSIwIiB5PSIwIiB3aWR0aD0iOTAwIiBoZWlnaHQ9IjU1MCIgZmlsbD0iI2Y4ZmFmYyIvPgogIDx0ZXh0IHg9IjQ1MCIgeT0iNDAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMjQiIGZvbnQtd2VpZ2h0PSJib2xkIiBmaWxsPSIjMGYxNzJhIj5TdHJhaW4gR2F1Z2UgaW4gYSBXaGVhdHN0b25lIEJyaWRnZTwvdGV4dD4KCiAgPCEtLSBsb2FkIGNlbGwgYm9keSAtLT4KICA8cmVjdCB4PSI4MCIgeT0iMTIwIiB3aWR0aD0iMjIwIiBoZWlnaHQ9IjcwIiByeD0iMTAiIGZpbGw9IiNjYmQ1ZTEiIHN0cm9rZT0iIzMzNDE1NSIgc3Ryb2tlLXdpZHRoPSIzIi8+CiAgPHRleHQgeD0iMTkwIiB5PSIxMDUiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTMiIGZvbnQtd2VpZ2h0PSJib2xkIj5Mb2FkIENlbGwgQm9keTwvdGV4dD4KICA8bGluZSB4MT0iMTIwIiB5MT0iMTQ1IiB4Mj0iMjYwIiB5Mj0iMTQ1IiBzdHJva2U9IiNkYzI2MjYiIHN0cm9rZS13aWR0aD0iMyIvPgogIDxsaW5lIHgxPSIxMjAiIHkxPSIxNjUiIHgyPSIyNjAiIHkyPSIxNjUiIHN0cm9rZT0iI2RjMjYyNiIgc3Ryb2tlLXdpZHRoPSIzIi8+CiAgPHRleHQgeD0iMTkwIiB5PSIyMTUiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTIiPkJvbmRlZCBzdHJhaW4gZ2F1Z2VzICjDlzQpPC90ZXh0PgoKICA8cGF0aCBkPSJNMTkwLDEyMCBMMTkwLDgwIiBzdHJva2U9IiMwZjE3MmEiIHN0cm9rZS13aWR0aD0iNCIgbWFya2VyLWVuZD0idXJsKCNkb3duKSIvPgogIDx0ZXh0IHg9IjE5MCIgeT0iNjUiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTMiIGZvbnQtd2VpZ2h0PSJib2xkIj5BcHBsaWVkIEZvcmNlIChGKTwvdGV4dD4KCiAgPGRlZnM+CiAgICA8bWFya2VyIGlkPSJkb3duIiBtYXJrZXJXaWR0aD0iMTAiIG1hcmtlckhlaWdodD0iMTAiIHJlZlg9IjUiIHJlZlk9IjgiIG9yaWVudD0iYXV0byI+CiAgICAgIDxwYXRoIGQ9Ik0wLDAgTDEwLDAgTDUsMTAgWiIgZmlsbD0iIzBmMTcyYSIvPgogICAgPC9tYXJrZXI+CiAgPC9kZWZzPgoKICA8IS0tIEJyaWRnZSBkaWFtb25kIC0tPgogIDxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKDQ4MCwyNjApIj4KICAgIDxwb2x5Z29uIHBvaW50cz0iMTIwLDAgMjQwLDEyMCAxMjAsMjQwIDAsMTIwIiBmaWxsPSJub25lIiBzdHJva2U9IiMzMzQxNTUiIHN0cm9rZS13aWR0aD0iMyIvPgogICAgPHRleHQgeD0iMTE1IiB5PSItMTUiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTIiIGZvbnQtd2VpZ2h0PSJib2xkIj4rRXhjaXRhdGlvbjwvdGV4dD4KICAgIDx0ZXh0IHg9IjExNSIgeT0iMjcwIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjEyIiBmb250LXdlaWdodD0iYm9sZCI+4oiSRXhjaXRhdGlvbjwvdGV4dD4KICAgIDx0ZXh0IHg9Ii00MCIgeT0iMTI1IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjEyIiBmb250LXdlaWdodD0iYm9sZCI+4oiSU2lnbmFsPC90ZXh0PgogICAgPHRleHQgeD0iMjcwIiB5PSIxMjUiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTIiIGZvbnQtd2VpZ2h0PSJib2xkIj4rU2lnbmFsPC90ZXh0PgoKICAgIDwhLS0gcmVzaXN0b3IgemlnemFncyBvbiBlYWNoIHNpZGUgLS0+CiAgICA8cGF0aCBkPSJNMTIwLDEwIEwxMDAsMzAgTDE0MCw1MCBMMTAwLDcwIEwxMjAsOTAiIHN0cm9rZT0iI2RjMjYyNiIgc3Ryb2tlLXdpZHRoPSIzIiBmaWxsPSJub25lIi8+CiAgICA8cGF0aCBkPSJNMjMwLDEyMCBMMjEwLDEwMCBMMjMwLDgwIEwyMTAsNjAgTDIzMCw0MCIgc3Ryb2tlPSIjZGMyNjI2IiBzdHJva2Utd2lkdGg9IjMiIGZpbGw9Im5vbmUiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDAsMTApIi8+CiAgICA8cGF0aCBkPSJNMTIwLDIzMCBMMTAwLDIxMCBMMTQwLDE5MCBMMTAwLDE3MCBMMTIwLDE1MCIgc3Ryb2tlPSIjZGMyNjI2IiBzdHJva2Utd2lkdGg9IjMiIGZpbGw9Im5vbmUiLz4KICAgIDxwYXRoIGQ9Ik0xMCwxMjAgTDMwLDEwMCBMMTAsODAgTDMwLDYwIEwxMCw0MCIgc3Ryb2tlPSIjZGMyNjI2IiBzdHJva2Utd2lkdGg9IjMiIGZpbGw9Im5vbmUiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDAsMTApIi8+CgogICAgPGNpcmNsZSBjeD0iMTIwIiBjeT0iMTIwIiByPSI0IiBmaWxsPSIjMGYxNzJhIi8+CiAgPC9nPgoKICA8bGluZSB4MT0iMzAwIiB5MT0iMTUwIiB4Mj0iNDgwIiB5Mj0iMjYwIiBzdHJva2U9IiM5NGEzYjgiIHN0cm9rZS13aWR0aD0iMiIgc3Ryb2tlLWRhc2hhcnJheT0iNSw1Ii8+CgogIDx0ZXh0IHg9IjQ1MCIgeT0iNDgwIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjE0IiBmaWxsPSIjMzM0MTU1Ij7OlFIvUiA9IEdGIMK3IM61ICAg4oCUICAgdGlueSByZXNpc3RhbmNlIGNoYW5nZXMgYmVjb21lIGEgbWVhc3VyYWJsZSBicmlkZ2Ugb3V0cHV0IHZvbHRhZ2U8L3RleHQ+CiAgPHRleHQgeD0iNDUwIiB5PSI1MjAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZpbGw9IiM0NzU1NjkiIGZvbnQtc2l6ZT0iMTUiIGZvbnQtc3R5bGU9Iml0YWxpYyI+CiAgICBGaWd1cmUgMTAg4oCUIEZvdXIgc3RyYWluIGdhdWdlcyBpbiBhIGJyaWRnZSBhbXBsaWZ5IHNlbnNpdGl2aXR5IGFuZCBjYW5jZWwgY29tbW9uLW1vZGUgdGVtcGVyYXR1cmUgZHJpZnQuCiAgPC90ZXh0Pgo8L3N2Zz4K)
*Figure 8 — Four strain gauges in a bridge amplify sensitivity and cancel common-mode temperature drift.*

The workhorse technology is the **strain gauge**: a resistive element whose resistance changes proportionally to the mechanical strain applied to it:

```
ΔR/R = GF · ε
```

Where `GF` is the gauge factor (typically ≈ 2 for metallic foil gauges) and `ε` is strain (dimensionless, ΔL/L). Because this resistance change is tiny, strain gauges are almost never read individually — they are arranged in a **Wheatstone bridge** (commonly a full bridge, four active gauges) so that the small resistance imbalance produces a measurable differential voltage, and so that unwanted effects (like temperature-driven resistance drift, which affects all four gauges equally) cancel out. A **load cell** is simply a precisely machined metal body with strain gauges bonded to it in a bridge configuration; **torque sensors** apply the same principle to a shaft under twist.

### 8.8 Vibration

Three related quantities describe the same mechanical motion, and each is preferred for different fault types:

- **Displacement** — how far something moves; best for low-frequency, high-amplitude issues (e.g., shaft misalignment, looseness).
- **Velocity** — how fast it moves; the traditional standard for general rotating-machinery health monitoring (ISO 10816/20816).
- **Acceleration** — the rate of change of velocity; best for detecting high-frequency events like bearing defects and gear mesh problems.

An **accelerometer** (commonly piezoelectric) is the physical sensing element in almost all industrial vibration monitoring; velocity and displacement are typically derived mathematically (by integration) from the raw acceleration signal. This section is foundational for the condition-monitoring and predictive-maintenance material that comes later in this repository.

---

## 9. Sensor Classification

```
Industrial Sensors
│
├── Contact           vs   Non-contact
├── Analog             vs   Digital
├── Active             vs   Passive
├── Direct             vs   Indirect
└── Smart / Networked
```

### 9.1 Active vs. Passive Sensors

- **Active** — generates its own electrical output from the measured phenomenon (e.g., a thermocouple, or certain piezoelectric applications).
- **Passive** — requires external excitation and changes an electrical parameter in response (e.g., RTD, strain gauge, potentiometer).

> This terminology varies between textbooks and industries — always confirm the convention being used in a given context rather than assuming.

---

## 10. Sensor Characteristics

These properties determine whether a sensor is *fit for purpose*, independent of what it measures:

| Property | Meaning |
|---|---|
| Range | Minimum to maximum measurable value |
| Span | Difference between upper and lower range values |
| Sensitivity | Output change per unit input change |
| Resolution | Smallest detectable change in the measured quantity |
| Accuracy | Closeness of the result to the true value |
| Precision | Closeness of repeated results to each other |
| Repeatability | Agreement between successive measurements under identical conditions |
| Reproducibility | Agreement between measurements taken under different conditions |
| Linearity | How closely output tracks a straight-line relationship to input |
| Hysteresis | Difference in output depending on direction of change (rising vs. falling) |
| Dead band | Range of input change that produces no output change |
| Threshold | Minimum input required to produce a detectable output |
| Drift | Slow change in output over time under constant input |
| Response time | How quickly output follows a change in input |
| Stability | Ability to maintain performance over time and conditions |

### 10.1 Accuracy vs. Precision

```
High accuracy, high precision:   ● ● ●  (tight cluster, centered)
High accuracy, low precision:    ●   ●  ●  (scattered, but centered)
Low accuracy, high precision:    ●●●     (tight cluster, off-center)
Low accuracy, low precision:     ●  ●   ● (scattered, off-center)
```

*A sensor can be highly precise and still wrong — precision alone is not proof of correctness.*

### 10.2 Range, Span, and Resolution — Worked Example

```
Pressure transmitter:
Range = 0–10 bar
Lower Range Value (LRV) = 0 bar
Upper Range Value (URV) = 10 bar
Span = URV − LRV = 10 bar
```

Resolution depends on the analog-to-digital converter behind it. A 12-bit ADC has `2¹² = 4096` discrete steps across its full input span:

```
Resolution = Span / 2ⁿ = 10 bar / 4096 ≈ 0.00244 bar per step (≈ 2.44 mbar)
```

Doubling the ADC resolution to 16-bit (`2¹⁶ = 65,536` steps) improves this to `10 / 65536 ≈ 0.000153 bar per step` — a 16× improvement for 4 extra bits. This is why the transmitter's internal ADC resolution, not just the sensing element itself, sets a hard floor on how finely a measurement can be reported. Note also that **resolution is not accuracy** — a 16-bit ADC can resolve extremely fine steps while still being wildly inaccurate if the sensing element itself is poorly calibrated or drifting.

### 10.2.1 Accuracy Specification — Reading the Fine Print

Instrument accuracy is usually specified one of two ways, and confusing them leads to real specification errors:

- **% of span** — a fixed absolute error across the whole range. A ±0.1% of span error on a 0–10 bar transmitter is always ±0.01 bar, whether the reading is 1 bar or 9 bar.
- **% of reading** — the error scales with the actual value. A ±0.1% of reading error at 1 bar is ±0.001 bar, but at 9 bar it is ±0.009 bar.

A transmitter spec'd "% of span" will look proportionally worse at low readings near the bottom of its range — this is one reason instruments are usually selected so the normal operating point sits well above the bottom of the calibrated range, not right at it.

### 10.3 Sensitivity

*How much the output changes when the input changes.*

```
Temperature ↑ 1 °C
       ↓
Resistance ↑ 0.385 Ω     (typical Pt100 RTD)
```

### 10.4 Linearity

```
Ideal response:              Actual response:
     /                            _/
    /                            /
   /                           _/
  /
 /
```

Real sensors rarely follow a perfect mathematical relationship — this is why transmitters and PLCs often apply linearization or scaling.

### 10.5 Hysteresis

```
Output
  ↑
  │     /──── (decreasing input)
  │    /
  │───/  (increasing input)
  │
  └──────────→ Input
```

The same input value can produce two different outputs depending on whether the input was rising or falling into that value.

---

## 11. Response Time and Dynamic Measurement

```
Physical change → Sensor response → Signal conditioning → Transmission → PLC scan → Control action
```

Every one of those steps adds delay. A process that changes faster than the measurement chain can respond will always appear "smoothed" or lagged on the HMI — and a controller reacting to a lagged measurement can overshoot or oscillate.

### 11.1 Static vs. Dynamic Measurement

| | Static | Dynamic |
|---|---|---|
| Process behavior | Slowly changing | Rapidly changing |
| Priority | Accuracy | Response time |
| Example | Tank temperature | Motor vibration |
| Example | Tank level | High-speed position |

---

## 12. Signal Conditioning

Raw sensor output is frequently too small, too noisy, or too nonlinear to use directly. Signal conditioning bridges the gap.

```
Sensor → Tiny / noisy signal → Amplifier → Filter → Isolation → Standard signal
```

Common signal-conditioning functions: amplification, filtering, isolation, linearization, excitation (powering passive sensors like RTDs and strain gauges), bridge circuits, and analog-to-digital conversion.

---

## 13. Standard Industrial Signals

| Category | Examples |
|---|---|
| Voltage | 0–10 V, ±10 V, 1–5 V |
| Current | 4–20 mA |
| Discrete | 24 V DC |
| Digital communication | HART, Modbus, IO-Link, industrial Ethernet |

Protocol-level detail (Modbus registers, HART commands, Ethernet/IP, etc.) is deliberately deferred to a later networking chapter — here the goal is only to recognize *why* these signal types exist.

### 13.1 Why 4–20 mA Became the Industry Standard

![4-20mA current loop wiring diagram](data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAxMDAwIDUwMCIgZm9udC1mYW1pbHk9IlNlZ29lIFVJLCBBcmlhbCwgc2Fucy1zZXJpZiI+CiAgPHJlY3QgeD0iMCIgeT0iMCIgd2lkdGg9IjEwMDAiIGhlaWdodD0iNTAwIiBmaWxsPSIjZjhmYWZjIi8+CiAgPHRleHQgeD0iNTAwIiB5PSI0MCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIyNCIgZm9udC13ZWlnaHQ9ImJvbGQiIGZpbGw9IiMwZjE3MmEiPjItV2lyZSA04oCTMjAgbUEgTG9vcC1Qb3dlcmVkIFRyYW5zbWl0dGVyPC90ZXh0PgoKICA8IS0tIFBMQyBwb3dlciBzdXBwbHkgLS0+CiAgPHJlY3QgeD0iNjAiIHk9IjE1MCIgd2lkdGg9IjE1MCIgaGVpZ2h0PSI5MCIgcng9IjgiIGZpbGw9IiNhNzhiZmEiIHN0cm9rZT0iIzRjMWQ5NSIgc3Ryb2tlLXdpZHRoPSIzIi8+CiAgPHRleHQgeD0iMTM1IiB5PSIxOTAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTMiIGZvbnQtd2VpZ2h0PSJib2xkIj4yNCBWREM8L3RleHQ+CiAgPHRleHQgeD0iMTM1IiB5PSIyMTAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTMiIGZvbnQtd2VpZ2h0PSJib2xkIj5Qb3dlciBTdXBwbHk8L3RleHQ+CgogIDwhLS0gVHJhbnNtaXR0ZXIgLS0+CiAgPHJlY3QgeD0iNDIwIiB5PSIxNTAiIHdpZHRoPSIxODAiIGhlaWdodD0iOTAiIHJ4PSI4IiBmaWxsPSIjZmI5MjNjIiBzdHJva2U9IiM3YzJkMTIiIHN0cm9rZS13aWR0aD0iMyIvPgogIDx0ZXh0IHg9IjUxMCIgeT0iMTkwIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjEzIiBmb250LXdlaWdodD0iYm9sZCI+RmllbGQ8L3RleHQ+CiAgPHRleHQgeD0iNTEwIiB5PSIyMTAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTMiIGZvbnQtd2VpZ2h0PSJib2xkIj5UcmFuc21pdHRlcjwvdGV4dD4KCiAgPCEtLSBQTEMgYW5hbG9nIGlucHV0IC0tPgogIDxyZWN0IHg9Ijc5MCIgeT0iMTUwIiB3aWR0aD0iMTUwIiBoZWlnaHQ9IjkwIiByeD0iOCIgZmlsbD0iIzIyZDNlZSIgc3Ryb2tlPSIjMGU3NDkwIiBzdHJva2Utd2lkdGg9IjMiLz4KICA8dGV4dCB4PSI4NjUiIHk9IjE5MCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMyIgZm9udC13ZWlnaHQ9ImJvbGQiPlBMQzwvdGV4dD4KICA8dGV4dCB4PSI4NjUiIHk9IjIxMCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMyIgZm9udC13ZWlnaHQ9ImJvbGQiPkFuYWxvZyBJbnB1dDwvdGV4dD4KCiAgPCEtLSB3aXJlcyAtLT4KICA8bGluZSB4MT0iMjEwIiB5MT0iMTc1IiB4Mj0iNDIwIiB5Mj0iMTc1IiBzdHJva2U9IiNkYzI2MjYiIHN0cm9rZS13aWR0aD0iNCIvPgogIDxsaW5lIHgxPSI2MDAiIHkxPSIxNzUiIHgyPSI3OTAiIHkyPSIxNzUiIHN0cm9rZT0iI2RjMjYyNiIgc3Ryb2tlLXdpZHRoPSI0Ii8+CiAgPGxpbmUgeDE9IjIxMCIgeTE9IjIxNSIgeDI9IjYwMCIgeTI9IjIxNSIgc3Ryb2tlPSIjMWQ0ZWQ4IiBzdHJva2Utd2lkdGg9IjQiLz4KICA8bGluZSB4MT0iNjAwIiB5MT0iMjE1IiB4Mj0iNzkwIiB5Mj0iMjE1IiBzdHJva2U9IiMxZDRlZDgiIHN0cm9rZS13aWR0aD0iNCIvPgoKICA8IS0tIHNodW50IHJlc2lzdG9yIGF0IFBMQyBpbnB1dCAtLT4KICA8cmVjdCB4PSI3NDAiIHk9IjE2NSIgd2lkdGg9IjMwIiBoZWlnaHQ9IjYwIiBmaWxsPSIjZmFjYzE1IiBzdHJva2U9IiM4NTRkMGUiIHN0cm9rZS13aWR0aD0iMiIvPgogIDx0ZXh0IHg9Ijc1NSIgeT0iMjUwIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjExIj4yNTAgzqkgc2h1bnQ8L3RleHQ+CgogIDx0ZXh0IHg9IjMxNSIgeT0iMTYwIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjEyIiBmaWxsPSIjZGMyNjI2Ij4rIGxvb3A8L3RleHQ+CiAgPHRleHQgeD0iNDAwIiB5PSIyMzUiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTIiIGZpbGw9IiMxZDRlZDgiPuKIkiBsb29wIChzaWduYWwgcmV0dXJuKTwvdGV4dD4KCiAgPHRleHQgeD0iNTAwIiB5PSIyOTAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTQiIGZpbGw9IiMzMzQxNTUiPlNhbWUgdHdvIHdpcmVzIGNhcnJ5IGJvdGggcG93ZXIgKHRvIHRoZSB0cmFuc21pdHRlcikgYW5kIHRoZSA04oCTMjAgbUEgc2lnbmFsIChiYWNrIHRvIHRoZSBQTEMpPC90ZXh0PgoKICA8IS0tIGN1cnJlbnQgc2NhbGUgLS0+CiAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMTUwLDM0MCkiPgogICAgPGxpbmUgeDE9IjAiIHkxPSIwIiB4Mj0iNzAwIiB5Mj0iMCIgc3Ryb2tlPSIjMzM0MTU1IiBzdHJva2Utd2lkdGg9IjMiLz4KICAgIDxsaW5lIHgxPSIwIiB5MT0iLTEwIiB4Mj0iMCIgeTI9IjEwIiBzdHJva2U9IiMzMzQxNTUiIHN0cm9rZS13aWR0aD0iMyIvPgogICAgPGxpbmUgeDE9IjM1MCIgeTE9Ii0xMCIgeDI9IjM1MCIgeTI9IjEwIiBzdHJva2U9IiMzMzQxNTUiIHN0cm9rZS13aWR0aD0iMyIvPgogICAgPGxpbmUgeDE9IjcwMCIgeTE9Ii0xMCIgeDI9IjcwMCIgeTI9IjEwIiBzdHJva2U9IiMzMzQxNTUiIHN0cm9rZS13aWR0aD0iMyIvPgogICAgPHRleHQgeD0iMCIgeT0iMzUiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTMiIGZvbnQtd2VpZ2h0PSJib2xkIj40IG1BICgwJSk8L3RleHQ+CiAgICA8dGV4dCB4PSIzNTAiIHk9IjM1IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjEzIiBmb250LXdlaWdodD0iYm9sZCI+MTIgbUEgKDUwJSk8L3RleHQ+CiAgICA8dGV4dCB4PSI3MDAiIHk9IjM1IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjEzIiBmb250LXdlaWdodD0iYm9sZCI+MjAgbUEgKDEwMCUpPC90ZXh0PgogICAgPHRleHQgeD0iMCIgeT0iLTIwIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjExIiBmaWxsPSIjZGMyNjI2Ij4ibGl2ZSB6ZXJvIiDigJQ8L3RleHQ+CiAgICA8dGV4dCB4PSIwIiB5PSItNSIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMSIgZmlsbD0iI2RjMjYyNiI+YmVsb3cgdGhpcyA9IGZhdWx0PC90ZXh0PgogIDwvZz4KCiAgPHRleHQgeD0iNTAwIiB5PSI0NjAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZpbGw9IiM0NzU1NjkiIGZvbnQtc2l6ZT0iMTUiIGZvbnQtc3R5bGU9Iml0YWxpYyI+CiAgICBGaWd1cmUgMTEg4oCUIFRoZSA0IG1BIGxpdmUgemVybyBlbmFibGVzIGJyb2tlbi13aXJlIGRldGVjdGlvbiB0aGF0IGEgMCBtQSBiYXNlbGluZSBuZXZlciBjb3VsZC4KICA8L3RleHQ+Cjwvc3ZnPgo=)
*Figure 9 — The 4 mA live zero enables broken-wire detection that a 0 mA baseline never could.*

```
4 mA  → 0 % of range
12 mA → 50 % of range
20 mA → 100 % of range
```

```
4 mA                12 mA                 20 mA
 │────────────────────│────────────────────│
 0%                  50%                  100%
```

The signal deliberately starts at **4 mA instead of 0 mA**. This "live zero" gives several practical advantages:

- **Broken-wire detection** — if the loop reads 0 mA, that's a fault, not a valid "0%" reading.
- **Powering two-wire transmitters** — the loop current itself can power the field device.
- **Improved fault diagnosis** — out-of-range currents (below 4 mA or above 20 mA) clearly indicate a problem rather than an extreme process value.

### 13.2 Two-Wire, Three-Wire, and Four-Wire Instruments

```
2-wire:  Power + Signal (shared conductors)
3-wire:  Power / Common / Signal
4-wire:  Power pair + Signal pair (fully separate)
```

Two-wire loop-powered transmitters are the most common in the field because they minimize cabling — the same two wires carry both power and the 4–20 mA signal.

---

## 14. Electrical Isolation and Noise

```
Sensor side │ [Isolation] │ PLC side
```

Isolation protects against ground loops, differing electrical potentials between devices, and noise coupling — preserving signal integrity between the field and the control system.

### 14.1 Noise and Signal Integrity

Contributing factors: electrical noise, electromagnetic interference (EMI), poor shielding, improper grounding, unsuitable cable types, and poor routing near high-power cabling.

```
Sensor ─────────────── PLC
        ↑
     NOISE
     ~~~~~
```

Mitigations include shielded and twisted-pair cabling, proper single-point grounding, physical separation from power cabling, and appropriate filtering.

### 14.2 Ground Loops

```
Instrument A
     │
     ├──── signal ──── PLC
     │                  │
     └──── ground ──────┘
```

When two ground references sit at slightly different potentials, unwanted current can flow through the signal path itself, corrupting the measurement — a classic and frustrating field problem.

---

## 15. Measurement Errors and Uncertainty

### 15.1 Types of Error

- **Systematic error** — a consistent, repeatable bias (e.g., a miscalibrated sensor that always reads 2° high).
- **Random error** — unpredictable variation from one reading to the next.
- **Gross error** — a mistake by a human, instrument, or process (e.g., wrong wiring, wrong range configured).

```
Measured result = True/reference value + Error
```

### 15.2 Uncertainty

> A measurement is not simply a number. It carries uncertainty.

NIST treats a measurement result and its associated uncertainty as inseparable, and stresses that traceability alone does not guarantee a measurement is fit for its intended purpose. A properly reported measurement therefore looks like `73.4 °C ± 0.3 °C`, not just `73.4 °C`.

Formal uncertainty analysis (following the internationally recognized *Guide to the Expression of Uncertainty in Measurement*, GUM) breaks uncertainty into two evaluation methods, not two different *kinds* of error:

- **Type A** — evaluated statistically, from a series of repeated observations. If `n` readings are taken with standard deviation `s`, the standard uncertainty of the *mean* is:

  ```
  u_A = s / √n
  ```

  This is why averaging repeated readings genuinely reduces uncertainty — but only up to the point where systematic effects (Type B) dominate.

- **Type B** — evaluated by any other means: manufacturer accuracy specifications, calibration certificates, published physical constants, or engineering judgment. A transmitter's datasheet accuracy figure is a Type B contribution, not a Type A one, because it wasn't derived from your own repeated measurements.

Individual uncertainty contributions are combined — not simply added — using a **root-sum-of-squares (RSS)** relationship, because independent error sources partially cancel rather than stacking directly:

```
u_combined = √(u₁² + u₂² + u₃² + ...)
```

Finally, the **expanded uncertainty** scales the combined uncertainty by a coverage factor `k` (commonly `k = 2`, giving approximately a 95% confidence interval for a normal distribution):

```
U = k · u_combined
```

So a result reported as `73.4 °C ± 0.6 °C (k = 2)` means there is roughly 95% confidence the true value lies within that band — this is the rigorous version of the "measurement is not a fact" idea introduced in §1.

---

## 16. Calibration, Verification, and Adjustment

```
Reference standard → Compare → Instrument indication → Determine error → Document result
```

**Calibration is not the same as adjustment.** NIST explicitly distinguishes calibration, adjustment, and verification/testing as separate activities:

| Activity | Main Purpose |
|---|---|
| **Calibration** | Establish the relationship between a reference and the instrument's indication |
| **Verification** | Check whether an instrument meets stated requirements |
| **Adjustment** | Physically modify the instrument to improve its performance |

![Instrument being compared against a reference standard in a calibration setting](data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAxMDAwIDUwMCIgZm9udC1mYW1pbHk9IlNlZ29lIFVJLCBBcmlhbCwgc2Fucy1zZXJpZiI+CiAgPHJlY3QgeD0iMCIgeT0iMCIgd2lkdGg9IjEwMDAiIGhlaWdodD0iNTAwIiBmaWxsPSIjZmVmY2U4Ii8+CiAgPHRleHQgeD0iNTAwIiB5PSI0MCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIyNCIgZm9udC13ZWlnaHQ9ImJvbGQiIGZpbGw9IiM0MjIwMDYiPkNhbGlicmF0aW9uOiBDb21wYXJpbmcgQWdhaW5zdCBhIFRyYWNlYWJsZSBSZWZlcmVuY2U8L3RleHQ+CgogIDwhLS0gUmVmZXJlbmNlIHN0YW5kYXJkIC0tPgogIDxyZWN0IHg9IjgwIiB5PSIxMzAiIHdpZHRoPSIyMjAiIGhlaWdodD0iMTQwIiByeD0iMTAiIGZpbGw9IiNiYmY3ZDAiIHN0cm9rZT0iIzE2NjUzNCIgc3Ryb2tlLXdpZHRoPSIzIi8+CiAgPHRleHQgeD0iMTkwIiB5PSIxOTAiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTYiIGZvbnQtd2VpZ2h0PSJib2xkIj5SZWZlcmVuY2U8L3RleHQ+CiAgPHRleHQgeD0iMTkwIiB5PSIyMTIiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTYiIGZvbnQtd2VpZ2h0PSJib2xkIj5TdGFuZGFyZDwvdGV4dD4KICA8dGV4dCB4PSIxOTAiIHk9IjI0MCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMiI+KHRyYWNlYWJsZSB0byBTSSk8L3RleHQ+CgogIDwhLS0gRFVUIC0tPgogIDxyZWN0IHg9IjcwMCIgeT0iMTMwIiB3aWR0aD0iMjIwIiBoZWlnaHQ9IjE0MCIgcng9IjEwIiBmaWxsPSIjZmVkN2FhIiBzdHJva2U9IiM5YTM0MTIiIHN0cm9rZS13aWR0aD0iMyIvPgogIDx0ZXh0IHg9IjgxMCIgeT0iMTkwIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjE2IiBmb250LXdlaWdodD0iYm9sZCI+RGV2aWNlIFVuZGVyPC90ZXh0PgogIDx0ZXh0IHg9IjgxMCIgeT0iMjEyIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjE2IiBmb250LXdlaWdodD0iYm9sZCI+VGVzdCAoRFVUKTwvdGV4dD4KICA8dGV4dCB4PSI4MTAiIHk9IjI0MCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMiI+KGZpZWxkIGluc3RydW1lbnQpPC90ZXh0PgoKICA8IS0tIGNvbXBhcmlzb24gbm9kZSAtLT4KICA8Y2lyY2xlIGN4PSI1MDAiIGN5PSIyMDAiIHI9IjcwIiBmaWxsPSIjZmVmOWMzIiBzdHJva2U9IiM4NTRkMGUiIHN0cm9rZS13aWR0aD0iMyIvPgogIDx0ZXh0IHg9IjUwMCIgeT0iMTk1IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjE1IiBmb250LXdlaWdodD0iYm9sZCI+Q29tcGFyZTwvdGV4dD4KICA8dGV4dCB4PSI1MDAiIHk9IjIxNSIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxNSIgZm9udC13ZWlnaHQ9ImJvbGQiPnJlYWRpbmdzPC90ZXh0PgoKICA8bGluZSB4MT0iMzAwIiB5MT0iMjAwIiB4Mj0iNDMwIiB5Mj0iMjAwIiBzdHJva2U9IiM0MjIwMDYiIHN0cm9rZS13aWR0aD0iMyIgbWFya2VyLWVuZD0idXJsKCNhMykiLz4KICA8bGluZSB4MT0iNTcwIiB5MT0iMjAwIiB4Mj0iNzAwIiB5Mj0iMjAwIiBzdHJva2U9IiM0MjIwMDYiIHN0cm9rZS13aWR0aD0iMyIgbWFya2VyLWVuZD0idXJsKCNhMykiLz4KICA8ZGVmcz4KICAgIDxtYXJrZXIgaWQ9ImEzIiBtYXJrZXJXaWR0aD0iMTAiIG1hcmtlckhlaWdodD0iMTAiIHJlZlg9IjgiIHJlZlk9IjUiIG9yaWVudD0iYXV0byI+CiAgICAgIDxwYXRoIGQ9Ik0wLDAgTDEwLDUgTDAsMTAgWiIgZmlsbD0iIzQyMjAwNiIvPgogICAgPC9tYXJrZXI+CiAgPC9kZWZzPgoKICA8bGluZSB4MT0iNTAwIiB5MT0iMjcwIiB4Mj0iNTAwIiB5Mj0iMzYwIiBzdHJva2U9IiM0MjIwMDYiIHN0cm9rZS13aWR0aD0iMyIgbWFya2VyLWVuZD0idXJsKCNhMykiLz4KICA8cmVjdCB4PSIzMzAiIHk9IjM2MCIgd2lkdGg9IjM0MCIgaGVpZ2h0PSI5MCIgcng9IjEwIiBmaWxsPSIjZmVjYWNhIiBzdHJva2U9IiM3ZjFkMWQiIHN0cm9rZS13aWR0aD0iMyIvPgogIDx0ZXh0IHg9IjUwMCIgeT0iMzk1IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjE1IiBmb250LXdlaWdodD0iYm9sZCI+RGV0ZXJtaW5lIEVycm9yPC90ZXh0PgogIDx0ZXh0IHg9IjUwMCIgeT0iNDE4IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjEzIj7ihpIgRG9jdW1lbnQgY2FsaWJyYXRpb24gY2VydGlmaWNhdGU8L3RleHQ+CgogIDx0ZXh0IHg9IjUwMCIgeT0iNDgwIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmaWxsPSIjNTc1MzRlIiBmb250LXNpemU9IjE1IiBmb250LXN0eWxlPSJpdGFsaWMiPgogICAgRmlndXJlIDYg4oCUIENhbGlicmF0aW9uIGNvbXBhcmVzIGEgRFVUIGFnYWluc3QgYSB0cmFjZWFibGUgcmVmZXJlbmNlOyB0aGUgcmVzdWx0IGlzIGRvY3VtZW50ZWQsIHdpdGggb3Igd2l0aG91dCBhZGp1c3RtZW50LgogIDwvdGV4dD4KPC9zdmc+Cg==)
*Figure 10 — Calibration compares a device under test (DUT) against a traceable reference; the result is documented, whether or not any adjustment follows.*

### 16.1 Traceability

```
Industrial Instrument
        ↓
  Working Standard
        ↓
  Reference Standard
        ↓
National / International Standard
        ↓
        SI
```

NIST defines metrological traceability as a documented, **unbroken chain of calibrations**, where each link in the chain contributes its own uncertainty to the final result. A plant-floor transmitter is only as trustworthy as the weakest link in that chain.

---

## 17. Measurement Loop in a Real Factory — Tank Temperature Control

```
Tank
 ↓
RTD
 ↓
Temperature transmitter
 ↓
4–20 mA
 ↓
PLC analog input
 ↓
PLC
 ↓
Control algorithm
 ↓
Output
 ↓
Control valve / heater
 ↓
Tank  (loop closes back to RTD)
```

### 17.1 Zooming Into the Signal Journey

```
Temperature
    ↓
RTD resistance
    ↓
Bridge / sensing circuit
    ↓
Transmitter electronics
    ↓
4–20 mA
    ↓
Analog input
    ↓
ADC
    ↓
Digital value
    ↓
PLC memory
    ↓
Control logic
```

This is the exact hand-off point into the PLC and electronics chapters later in this repository — everything before this line is instrumentation; everything after is computation.

---

## 18. Analog-to-Digital Conversion, Sampling, and Aliasing

```
Physical quantity → Analog signal → ADC → Digital number → PLC processing
```

Core ideas to carry forward (deeper ADC architecture — successive-approximation, sigma-delta, and so on — is reserved for a dedicated electronics chapter):

- **Sampling** — taking discrete snapshots of a continuous signal at fixed time intervals.
- **Resolution** — the smallest digital step the ADC can represent, in bits (see the worked example in §10.2).
- **Quantization error** — every real reading gets rounded to the nearest available digital step; the maximum possible error is half of one step (`± ½ LSB`), and this behaves like an unavoidable, irreducible noise floor added by digitization itself.
- **Aliasing** — sampling too slowly relative to how fast the signal changes produces an incorrect apparent signal.

### 18.1 The Nyquist–Shannon Sampling Theorem

A continuous signal can be perfectly reconstructed from its samples only if it is sampled at a rate **at least twice** the highest frequency component present in that signal:

```
f_sample ≥ 2 × f_signal_max
```

Violating this produces **aliasing** — a high-frequency signal component gets sampled so infrequently that it appears, in the sampled data, as a completely different (and lower) frequency. A classic industrial example: a vibration signal with real content at 150 Hz, sampled at only 200 Hz (well under the required 300 Hz Nyquist rate), will show up in the data as a false low-frequency component — potentially masking a real bearing fault or fabricating one that doesn't exist. This is why vibration monitoring systems specify sample rates far above the highest frequency of diagnostic interest, and why an **anti-aliasing filter** (a low-pass filter applied *before* the ADC) is standard practice: it removes frequency content above the Nyquist limit before sampling can misrepresent it, rather than trying to fix the problem after the fact — which is impossible, because once aliasing occurs the original information is genuinely lost.

---

## 19. Choosing the Right Instrument

```
What must be measured?
        ↓
Required range?
        ↓
Required accuracy?
        ↓
Required response time?
        ↓
Environment?
        ↓
Process connection?
        ↓
Signal type?
        ↓
Hazardous area classification?
        ↓
Maintenance requirements?
        ↓
Cost / lifecycle?
        ↓
Select instrument
```

This turns everything above from theory into an engineering decision — instrument selection is always a trade-off exercise, never a single "best" answer.

---

## 20. Measurement Failure — What Happens When the Sensor Lies?

Common failure modes: broken wire, sensor drift, short circuit, open circuit, noisy signal, wrong scaling, wrong configured range, calibration failure, installation error, and process disturbances that fool the sensing principle (e.g., foam confusing a level sensor).

> **A PLC can execute its logic perfectly and still control a process incorrectly if the measurement feeding it is wrong.**

This is one of the most important first-principles lessons in industrial automation: the controller trusts its inputs completely. It has no way to know a sensor is lying unless it is explicitly told to check.

---

## 21. Diagnostic Thinking

When a measurement looks wrong, trace it methodically through the chain rather than guessing:

```mermaid
flowchart TD
    A[Unexpected reading] --> B{Process actually\nat that value?}
    B -- Yes --> C[Not a measurement fault]
    B -- No --> D{Sensor itself OK?}
    D -- No --> E[Replace / repair sensor]
    D -- Yes --> F{Wiring intact?}
    F -- No --> G[Fix wiring / connections]
    F -- Yes --> H{Power to transmitter OK?}
    H -- No --> I[Restore power]
    H -- Yes --> J{Signal correct at I/O module?}
    J -- No --> K[Check transmitter output / loop]
    J -- Yes --> L{PLC tag scaling correct?}
    L -- No --> M[Fix scaling / engineering units]
    L -- Yes --> N{HMI displaying correctly?}
    N -- No --> O[Fix HMI binding / display config]
    N -- Yes --> C
```

*Figure 11 — A structured troubleshooting path: process → sensor → wiring → power → signal → I/O module → PLC tag → scaling → HMI.*

---

## 22. Case Study — Tank Level Discrepancy

**Symptom:** *Tank is physically 70% full, but the HMI shows 40%.*

Walk the chain systematically, and at each step, form a hypothesis you can actually test — not just a guess:

| Step | Check | How | Result rules out |
|---|---|---|---|
| 1. Physical | Confirm actual level | Manual gauge, sight glass, or dipstick | Confirms the fault is in the measurement chain, not the process itself |
| 2. Sensor | Mounting and sensing face | Visual inspection; check for buildup, foam, or misalignment (especially for radar/ultrasonic) | Sensor fouling or installation error |
| 3. Wiring | Terminal integrity | Visual + tug test on terminals; check for corrosion | Loose or corroded connection |
| 4. Signal at source | Actual transmitter output | Multimeter across the loop (or a HART handheld for digital diagnostics) — does it read ~13.6 mA for 70% (`4 + 0.16 × 16`)? | Transmitter fault vs. downstream fault |
| 5. PLC analog input | Module configuration | Compare configured input range (e.g., 4–20 mA) against actual wiring and expected engineering units | Misconfigured I/O module |
| 6. Scaling | PLC scaling formula | Check the raw-count-to-engineering-unit conversion in the program | Software scaling error |
| 7. HMI | Tag binding and display scaling | Confirm the HMI tag points to the correct PLC address and its own display scaling matches | HMI-side misconfiguration |

If step 4 already shows an incorrect transmitter output (say, 8 mA instead of ~13.6 mA), the fault lives upstream of the PLC entirely — no amount of PLC or HMI investigation will fix it, and continuing to "fix" scaling downstream would only mask a real field problem. This is exactly why the diagnostic walk must proceed **in order**, from the physical world inward — jumping straight to software is one of the most common wastes of time in instrumentation troubleshooting.

This case study is the practical payoff of the entire chapter: **engineering thinking, not memorization.** The same seven-step walk applies to almost any "the number looks wrong" problem in an industrial system.

---

## 23. Mini Laboratory Experiments

1. Measure temperature with an RTD and compare against a reference thermometer.
2. Observe a 4–20 mA loop directly with a multimeter across a shunt resistor.
3. Compare a 0–10 V signal against a 4–20 mA signal for the same physical quantity.
4. Introduce sensor noise (e.g., proximity to a noisy cable) and observe the effect of filtering.
5. Scale a raw analog PLC input into engineering units.
6. Calculate sensor error given a reference value and an indicated value.
7. Perform a basic calibration comparison between a field instrument and a reference standard.

---

## 24. Engineering Questions

1. Why does automation require measurement?
2. What is a measurand?
3. What is the difference between a sensor and a transducer?
4. Why is 4–20 mA so widely used in industry?
5. Why use 4 mA instead of 0 mA as the "zero" point?
6. What is calibration?
7. How does calibration differ from adjustment?
8. What is the difference between accuracy and precision?
9. What is hysteresis?
10. What is repeatability, and how does it differ from reproducibility?
11. What does "measurement uncertainty" mean?
12. What is metrological traceability?
13. Why is signal conditioning necessary before a signal reaches a PLC?
14. Why does ADC resolution matter for measurement quality?
15. What causes measurement noise, and how can it be reduced?
16. How can a sensor failure affect PLC control even if the PLC logic is correct?

---

## 25. Final Mental Model

```
REAL WORLD
    ↓
Physical phenomenon
    ↓
MEASUREMENT
    ↓
Sensor / Transducer
    ↓
SIGNAL
    ↓
Conditioning / Transmission
    ↓
DATA
    ↓
PLC / Controller
    ↓
DECISION
    ↓
ACTUATION
    ↓
REAL WORLD  ↺
```

> **Industrial automation begins with knowing what is happening in the physical world. Measurement turns physical reality into information that a control system can understand — and every layer that follows this chapter depends on that information being trustworthy.**

---

## Chapter Assets

This chapter's diagrams follow the repository's three-level visual strategy:

- **Mermaid diagrams** — embedded directly above, rendered natively by GitHub (chain diagrams, feedback loop, troubleshooting tree).
- **Embedded diagrams** — all 10 figures are embedded directly in this file as inline base64-encoded SVGs, so the chapter is fully self-contained: no `images/` folder or separate assets are required for them to display.
- **`animations/` folder** *(future work)* — interactive HTML/CSS/JS demonstrations for RTD response, 4–20 mA scaling, ADC sampling/aliasing, sensor response time, and signal filtering, hosted via GitHub Pages.

---

**Previous:** [04 — Industrial Processes and Systems](../04-industrial-processes-and-systems.md)
**Next:** [06 — Actuators and Final Control Elements](../06-actuators-and-final-control-elements.md)
