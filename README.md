# Nexys A7 VGA Penalty Kick Game
A custom FPGA game implemented in **VHDL** for the **Nexys A7** board.  
The player aims a penalty kick using onboard buttons, adjusts power using an oscillating power meter, and shoots toward the goal while a goalie dives left/center/right with pseudo‑random logic.

---

## 1. Expected Behavior of the Project
- Displays a **soccer field**, **goal**, **ball**, and **goalie** on a VGA monitor (800×600).
- Player uses **BTNL/BTNR/BTNU/BTND** to aim a crosshair.
- Player uses **BTNC** to shoot.
- A **power meter bar** oscillates and determines shot speed.
- A **pseudo‑random LFSR** chooses one of three goalie dives (left, center, right).
- A **collision detection system** determines whether the goalie blocks the shot.
- Displays **“GOAL!”** or **“MISS!”** in comic‑style text.
- A **60‑second timer** runs; total goals are displayed on the **7‑segment display**.
- **High score** is saved for the session.
- Game loops until timer expires.

### Required Attachments
- Nexys A7 board  
- VGA cable  
- Power supply  

### System Diagram  
![High-level system diagram showing VGA renderer, game logic FSM, input controller, and 7-segment driver](block.png)

---

## 2. Steps to Build & Run in Vivado
1. Create a new **RTL project** in Vivado.  
2. Add:
   - `penalty_top.vhd`
   - `vga_sync.vhd`
   - `nexys_a7_penalty.xdc`
3. Set `penalty_top` as the **top module**.
4. Generate bitstream.
5. Open **Hardware Manager**, program the Nexys board via USB.
6. Connect VGA monitor and press **reset** to start the game.

---

## 3. Inputs and Outputs (Nexys A7)

### Inputs
| Nexys Button | Function |
|--------------|----------|
| **BTNL** | Move aim left |
| **BTNR** | Move aim right |
| **BTNU** | Move aim up |
| **BTND** | Move aim down |
| **BTNC** | Shoot |

### Outputs
| Output | Function |
|--------|----------|
| **VGA (R,G,B,HS,VS)** | Full graphics output |
| **7‑segment display** | Shows current score or high score |
| **LEDs (optional)** | Can be used for debugging |

### Added / Modified Ports (Required by Assignment)
- Added button inputs for aim & shooting  
- Added VGA output ports  
- Added 7‑segment output ports  
- Updated `.xdc` constraints to include all above

---

## 4. Images / Videos of Project Running
![goal](goal.png)
![miss](miss.png)
[gameplay demo (Google Drive)](https://drive.google.com/file/d/11C3PiTIgpmEcDeDKwcHJblMV90sYGgqK/view?usp=drive_link)

---

## 5. Modifications Made (15 Points)

Our project was built using a minimal baseline consisting of:
- A standard **VGA timing generator** (`vga_sync.vhd`)
- A **blank top-level module template** with no game logic or rendering

Starting from this baseline, we significantly expanded the design into a complete, interactive hardware game.

### Major Additions and Modifications
- Designed and implemented a full **penalty kick game engine** using a frame-based FSM (AIM → SHOT → RESULT → GAMEOVER)
- Integrated **pseudo-random goalie behavior** using an 8-bit LFSR, including logic to prevent repeated dive directions
- Implemented **goalie dive motion** with bounded movement and target positions (left, center, right)
- Added **collision detection** between the ball and goalie using circle-vs-rectangle geometry
- Designed an **oscillating power meter** that determines shot velocity
- Added a **crosshair-based aiming system** controlled by pushbuttons
- Implemented a **60-second game timer** with persistent **high-score tracking**
- Created a **custom VGA text rendering system** for “GOAL!” and “MISS!” messages using a hardcoded font
- Designed a **7-segment display driver** with digit multiplexing and decoding
- Built a full **VGA renderer** to draw the field, goal, goalie, ball, UI elements, and text in real time

### Starter Code Used
- `vga_sync.vhd` — standard Digilent VGA timing module  
- All other game logic, rendering, control, and display functionality was written or substantially rewritten by the project team.

---

## 6. Project Summary, Roles, Timeline, Difficulties

### Roles
- **Aayush** – Game logic, goalie movement, collision detection  
- **Ardit** – Documentation, Vivado setup, constraints file, testing  
- **Anthony** – Graphics, ball sprite integration, UI elements  

### Timeline
- Week 1: Basic VGA output + field rendering  
- Week 2: Aim controls, ball physics, goalie movement  
- Week 3: Collision detection & scoring  
- Week 4: Power meter, text graphics, 7‑segment display  
- Week 5: Final debugging + polishing + README  

### Difficulties & Solutions
- **Goalie vibrating at boundaries** → added clamping logic  
- **Goal deciding incorrectly** → refined collision box  
- **7‑segment showing hex instead of decimal** → fixed decoder + digit select  
- **Power meter too slow** → added speed constant  
- **Repeat goalie dives** → enforced no‑repeat logic  


