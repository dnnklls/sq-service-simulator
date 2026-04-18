# SQ EY Service Flow Simulator
### A350 Medium Haul — Cabin Service Workflow Simulator

---

## What It Is

A browser-based simulation tool built for Singapore Airlines cabin crew to model, visualise, and optimise Economy Class meal service flow on an A350 medium haul flight. No install required — runs entirely in a single HTML file.

---

## High-Level Functions

### 1. Cabin Generation
- Renders a to-scale Economy cabin grid based on configurable row count and seat layout (3-3-3, 2-4-2, 3-4-3)
- Populates pax occupancy randomly based on a load factor percentage (e.g. 85%)
- Seats are colour-coded live during simulation: idle → in-progress → served

### 2. Crew Assignment & Competency
- Add any number of crew members with custom names (IFS, LSS, FSS, etc.)
- Assign each crew member to a cabin zone: Forward (rows 1–14), Mid (15–27), or Aft (28–40)
- Set individual competency on a 1–5 scale — this applies a speed multiplier to all their timing calculations:
  - Expert (5) = 0.70× base time
  - Fast (4) = 0.85×
  - Average (3) = 1.00×
  - Slow (2) = 1.20×
  - Very Slow (1) = 1.50×

### 3. SPML (Special Meal) Handling
- Define special meals by row number and type (VGML, KSML, MOML, CHML, DBML, LFML, GFML, NLML, SFML, HNML)
- SPML rows incur a configurable additional time penalty on top of the normal meal service time
- SPML events are flagged in the activity log with row and meal type

### 4. Service Direction
- **FWD → AFT**: trolleys start from the front, work toward the tail
- **AFT → FWD**: trolleys start from the rear, work forward
- **Both**: crew split — alternating crew members work from opposite ends simultaneously, simulating a two-trolley service

### 5. Timing Engine
All timing parameters are user-configurable (in seconds):
| Parameter | Default | Description |
|---|---|---|
| Normal meal | 18s | Time to serve one row of pax |
| SPML delay | 35s | Additional time for a SPML row |
| Row movement | 4s | Trolley travel between rows |
| Trolley setup | 60s | Pre-service galley prep time |
| Drink service | 12s | Drink round component per row |
| Tray collection | 10s | Collection pass per row |

Each value is scaled by the individual crew member's competency multiplier.

### 6. Simulation Playback
- Animated cabin map updates in real time as rows are served
- Configurable simulation speed: 0.5× to 20×
- Elapsed time counter updates live

### 7. Timeline View (Gantt)
- Per-crew horizontal timeline showing three service phases:
  - **Setup** (trolley prep)
  - **Meal Service** (row-by-row distribution)
  - **Tray Collection** (reverse pass)
- Live cursor tracks current simulation time across all crew lanes
- Hover tooltips show exact start/end time of each phase segment

### 8. Stats & Logs
- **Total Pax Served**: sum of occupied seats across all zones
- **Service Duration**: total elapsed time to completion (driven by slowest crew)
- **SPML Handled**: count of special meal rows processed
- **Avg per Pax**: average service time per passenger
- **Crew Efficiency %**: actual performance vs ideal baseline
- **Bottleneck**: identifies the crew member who finishes last
- **Activity Log**: timestamped log of every key event (setup complete, SPML hit, service done)

---

## Use Cases

| Who | How They'd Use It |
|---|---|
| **IFS / In-Charge** | Plan zone assignments and direction before a flight; identify if crew competency gaps will cause timing issues |
| **Crew Trainers** | Demonstrate the time cost of SPML clusters, slow service speed, or poor zone coverage |
| **Planning / CCD** | Model service timelines for different load factors or aircraft configurations |
| **New Crew** | Understand how their position in the cabin and competency affects overall service flow |

---

## Current Limitations

- Pax occupancy is randomised per run — not based on actual load data
- SPML placement is manually entered by row, not imported from a meal manifest
- Crew zones are fixed to thirds of the cabin — no custom row range input yet
- No galley layout modelling (oven cycles, cart staging, chiller positions)
- Drink service is folded into the meal pass rather than modelled as a separate round
- No modelling of aisle blockage or crew crossing conflicts
- Single-aisle logic only — both aisles treated identically

---

## Future Improvements

### Short Term
- [ ] **Custom zone row ranges** — let each crew member define exact start/end rows instead of preset thirds
- [ ] **Save/load configs** — export and import crew + SPML setups as JSON for pre-flight planning
- [ ] **Seat map import** — paste a load sheet or CSV to populate actual pax occupancy instead of random generation
- [ ] **SPML manifest import** — enter all SPMLs at once from a text list (e.g. `12A VGML, 24C KSML`)
- [ ] **Separate drink round** — model pre-meal drinks and post-meal hot drinks as distinct service passes with their own timing
- [ ] **Replay mode** — step through the simulation frame by frame after it completes

### Medium Term
- [ ] **Galley modelling** — oven cycles, cart heating times, staging bottlenecks at the galley door
- [ ] **Aisle conflict detection** — flag when two crew members' trolleys would meet mid-cabin and model the delay
- [ ] **Crew fatigue factor** — service speed degrades slightly over time on longer runs
- [ ] **Multi-class support** — extend to J or F galley service flows on the same aircraft
- [ ] **Actual A350 seat map** — hardcode the real SQ A350 EY layout (rows 36–97 on SQ config) with accurate emergency exit row positions
- [ ] **PDF/PNG export** — export the timeline as a pre-flight briefing document

### Longer Term
- [ ] **Roster integration** — pull crew names and ranks directly from the Notion Flights/Crew database via API
- [ ] **Historical benchmarking** — log completed simulations and compare across flights/crew combinations
- [ ] **Mobile-optimised view** — usable on phone during pre-flight briefing
- [ ] **Collaborative mode** — IFS and crew can each configure their own settings and see a shared timeline
- [ ] **AI suggestions** — given crew competency and SPML distribution, auto-suggest optimal zone assignments and direction

---

*Built for SQ cabin crew operational planning. Single HTML file, no dependencies, runs offline.*
