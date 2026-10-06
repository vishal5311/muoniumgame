# Muonium Office Command 🏢⚡

> **Real-time 3D office strategy game** — Lead a Chennai AI creative and technology studio. Manage 20 staff across 5 divisions, deliver client briefs, hire talent, and scale your studio to ₹2,50,000.

![Three.js](https://img.shields.io/badge/Three.js-0.160.0-blue) ![Vanilla JS](https://img.shields.io/badge/JS-ES%20Modules-yellow) ![No Build Step](https://img.shields.io/badge/build-none-green)

---

## 🚀 How to Run

```bash
# Option 1: Python (built-in, recommended)
cd /path/to/Office_game
python3 -m http.server 8080
# Open http://localhost:8080

# Option 2: Node.js serve
npx serve .
# Open the URL shown

# Option 3: VS Code Live Server
# Right-click index.html → "Open with Live Server"
```

> ⚠️ Must be served over HTTP (not `file://`) because of ES Module importmaps and the GLTF loader.

---

## 🎮 Gameplay

### Goal
Grow studio treasury from ₹60,000 to ₹2,50,000 before going bankrupt or losing all reputation.

### Mechanics
| Mechanic | Detail |
|---|---|
| **Client Briefs** | Spawn on a timer; 10 max open. Each has division, pay, deadline |
| **Work Progress** | Staff work at `skill × energy_factor × focus_factor × lend_factor` per second |
| **Energy** | Drains while working (0.8/s), drains faster in Focus (2.4/s), idle regen +2/s |
| **Coffee Break** | Costs ₹300, staff walks to pantry, recovers +16 energy/s for 6 seconds |
| **Deep Focus** | 1.6× work multiplier, drains energy fast, auto-off below 8% energy |
| **Lend Staff** | Send a person to another division's pod (0.6× efficiency) |
| **Hire** | Recruit freelancers (₹8,000 each), max 30 staff total |
| **Upgrade** | Pay ₹2,000 to boost a staff member's skill by +0.15× (max 2.5×) |
| **Payroll** | ₹140/second continuously deducted |

### Win / Lose
- 🏆 **Win**: Cash ≥ ₹2,50,000
- 💀 **Lose**: Cash < ₹0 OR Reputation ≤ 0

---

## ⌨️ Keyboard Shortcuts

| Key | Action |
|---|---|
| `Space` | Pause / Resume |
| `1 / 2 / 4` | Set simulation speed |
| `F` | Toggle Deep Focus (selected person) |
| `C` | Send to Coffee Break (selected person) |
| `U` | Upgrade skill (selected person, ₹2,000) |
| `Esc` | Deselect person |
| `R` | Reset camera to isometric |
| `M` | Toggle mute |
| `T` | Toggle dark/light theme |
| `?` | Show keyboard shortcuts overlay |

---

## 🖱️ Camera Controls

| Input | Action |
|---|---|
| Left drag | Orbit around office |
| Right drag / Shift+drag | Pan camera |
| Scroll wheel | Zoom in/out |
| ISO button | Reset to isometric view |
| TOP button | Switch to top-down plan view |

---

## 🏆 Achievements (12 Total)

| Badge | Name | Condition |
|---|---|---|
| 📦 | First Delivery | Deliver your first brief |
| 🚀 | On a Roll | Deliver 10 briefs |
| 🏭 | Brief Machine | Deliver 20 briefs |
| 👑 | Legendary Reputation | Reach 100% reputation |
| 💰 | Six Figures | Accumulate ₹1,00,000 cash |
| ⚡ | The Flow State | 3+ staff in Deep Focus at once |
| 🧑‍🤝‍🧑 | Growing Fast | Hire 5 freelancers |
| 🎯 | Zero Fault Streak | 5 briefs delivered without a miss |
| ⬆ | Skill Architect | Upgrade any staff 3 times |
| 🌩️ | Crisis Manager | Survive a random event |
| ☕ | Caffeine Culture | Order 10 coffees total |
| 💎 | Enterprise Deal | Deliver a brief worth ₹4,000+ |

---

## ⚡ Random Events

8 random events can fire during gameplay:

- 🔌 **Power Cut** — All focus modes cancelled for 15 seconds
- 📝 **Scope Creep** — A random brief gets 40% more work added
- 📈 **Viral Moment** — +₹5,000 and +5% reputation bonus
- 🥤 **Energy Drink Sponsor** — All staff get +20 energy
- 💸 **Client Appreciation Bonus** — Free ₹3,000 tip from client
- 💻 **Equipment Failure** — One division works at half speed for 20 seconds
- 🏅 **Industry Award** — +10% reputation instantly
- ☕ **Free Coffee Day** — Low-energy staff automatically sent to pantry

---

## 📁 Project Structure

```
Office_game/
├── index.html          ← Complete self-contained game (HTML/CSS/JS)
├── assets/
│   └── models/         ← Drop custom GLB files here (see below)
└── README.md
```

---

## 🎨 Adding Custom 3D Models (GLB Format)

The game automatically falls back to procedural placeholders if model files are missing. To replace them with custom assets:

### File Names to Drop in `/assets/models/`
| File | Replaces |
|---|---|
| `person_studios.glb` | Studios division staff character |
| `person_gaming.glb` | Gaming division staff character |
| `person_tech.glb` | Tech division staff character |
| `person_media.glb` | Media division staff character |
| `person_ops.glb` | Ops division staff character |
| `desk.glb` | Workstation desk unit |
| `monitor.glb` | Computer monitors |
| `chair.glb` | Office chair |
| `pantry.glb` | Coffee pantry counter |
| `plant.glb` | Decorative plant |
| `wall_window.glb` | Window wall panel |
| `floor_tile.glb` | Floor tile |

### Export Guidelines (Blender → Three.js)
1. **Format**: GLB (binary GLTF), optionally Draco or Meshopt compressed
2. **File size**: Under 2 MB each
3. **Origin**: Place the model's foot/base at world origin (0,0,0)
4. **Facing direction**: Model should face +Z axis
5. **Scale**: Model height should be approximately 1.0–2.0 Blender units; the engine auto-normalizes
6. **Division colour material**: Name a material exactly `division_color` — the engine will tint it to the division's theme colour automatically
7. **Animations** (optional): Include clips named exactly `idle`, `walk`, and `work` — the engine blends them automatically

### Export Command (Blender CLI)
```bash
blender --background model.blend --python-expr "
import bpy
bpy.ops.export_scene.gltf(filepath='person_tech.glb', export_format='GLB', export_draco_mesh_compression_enable=True)
"
```

---

## 🏗️ Code Architecture

The code in `index.html` is organized into clear, numbered sections for easy extension:

| Section | Contents |
|---|---|
| `1. CONFIG` | Division data, staff templates, brief name pools, MODELS manifest |
| `2. SOUND ENGINE` | Web Audio API synthesis (no external audio files needed) |
| `3. ACHIEVEMENTS` | 12-achievement definitions with check functions |
| `4. RANDOM EVENTS` | 8-event catalogue with effect functions |
| `5. GAME STATE` | `freshState()`, `makeStaffPerson()`, `const State` |
| `6. HELPERS` | `fmtINR()`, `fmtTime()`, `logMessage()`, `project3DToScreen()`, `spawnFloatToast()` |
| `7. ACHIEVEMENT SYSTEM` | `checkAchievements()`, toast display, badge rendering |
| `8. RANDOM EVENT SYSTEM` | `showEventBanner()`, `triggerRandomEvent()` |
| `9. SIMULATION LOGIC` | `spawnBrief()`, `updateSimulation()`, `showEndModal()`, `resetGame()` |
| `10. HIRE SYSTEM` | `hireFreelancer()`, `renderHireTab()` |
| `11. THREE.JS SETUP` | Scene, OrthographicCamera, renderer, lighting |
| `12. MATERIALS & GEOMETRIES` | Shared material/geometry singletons |
| `13. ENVIRONMENT BUILD` | Floor, walls, windows, pods, pantry, plants |
| `14. CHARACTER BUILDER` | `createProceduralCharacter()`, GLTF loader with fallback, `normalizeModel()` |
| `15. CAMERA CONTROLS` | Orbit, pan, zoom, pointer events |
| `16. BILLBOARD SYSTEM` | In-world projected HTML labels for each character |
| `17. UI RENDERING` | `updateUI()`, `renderBriefs()`, `renderTeam()` |
| `18. EVENT LISTENERS` | All DOM and keyboard event wiring |
| `19. RENDER LOOP` | `requestAnimationFrame` loop, character animation, throttled UI updates |
| `20. INIT` | `startApp()` — boots everything in order |

---

## ⚙️ Performance Notes

- **Target**: 60 fps on a normal laptop and mobile (Chrome/Safari)
- `devicePixelRatio` capped at 2
- Shadow map capped at 1024×1024
- No post-processing effects
- UI DOM updates throttled to 5/second to avoid layout thrashing
- Shared geometries and materials (no per-character duplicates for static parts)

---

## 📄 License

MIT — Build, fork, extend, and ship freely.
