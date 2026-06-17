# ⚡ Thunder Math

**Zap the monsters with thunder before they reach you** — pick a mode, choose a difficulty, build big combos, and beat the boss!

Thunder Math is a fast, arcade-style math practice game for kids, rendered in **3D with three.js (WebGL)**. Every monster carries a question; tap the right answer to strike it down with lightning before it reaches the hero. The whole game is a **single HTML file** — no build step, no accounts, no data collected — and it's **designed for mobile screens** as well as desktop.

🎮 **[Play it now »](https://pasuay.github.io/thunder-math/)**

![Thunder Math title screen](screenshots/title.png)

---

## ✨ Features

- **Four practice modes**
  - ➕ **Add & Subtract** — within 20
  - ✖️ **Times Tables** — tables to 12
  - 🔟 **Tens & Ones** — place value to 100
  - ➡️ **Number Patterns** — skip counting
- **Four difficulty tiers** — Easy, Medium, Hard, and a **Boss** fight
- **Combo system & scoring** — answer fast and accurately to build big combos and earn up to ⭐⭐⭐ per level
- **Built on math-education research** — the difficulty ladder in each mode follows established fluency standards and peer-reviewed studies (count on → make ten → bridging → doubles, the array model for multiplication, base-ten blocks for place value, and more)
- **3D graphics in the browser** — built with **three.js / WebGL** for the scene and effects, with a 2D canvas HUD overlay
- **Mobile-screen compatible** — responsive layout, large touch-friendly tap targets, and a locked viewport so it plays great on phones and tablets (touch and mouse both supported)
- **Zero install** — a single HTML file; just open it or host it anywhere static

## 🕹️ How to play

1. Pick a **mode** (Add & Subtract, Times Tables, Tens & Ones, or Number Patterns).
2. Choose a **difficulty** — or jump straight into a **Boss** battle.
3. Monsters drift toward your hero, each showing a math problem.
4. **Tap the correct answer** to zap the highlighted monster with thunder.
5. Keep your hero's health up, build your combo, clear the stages, and defeat the boss!

<p align="center">
  <img src="screenshots/gameplay.png" alt="Gameplay — many monsters, one lightning strike" width="280">
  &nbsp;&nbsp;
  <img src="screenshots/thunderstorm.png" alt="THUNDERSTORM combo — chain lightning" width="280">
</p>

<p align="center"><sub><b>Left:</b> many monsters, one lightning strike &nbsp;·&nbsp; <b>Right:</b> the <b>THUNDERSTORM</b> combo — build a 5-answer streak to chain lightning across every monster on screen.</sub></p>

## 🛠️ Built with

- **HTML5** — a single `index.html` file, no build tooling
- **[three.js](https://threejs.org/) (r128) over WebGL** — `WebGLRenderer` + `PerspectiveCamera` for the 3D scene, loaded from a CDN
- **2D Canvas** — HUD and on-screen effects overlay
- **Web Audio API** — sound effects and music

> Note: three.js is loaded from a CDN, so an internet connection is needed the first time it runs.

## 🚀 Run it locally

No build step required. Either:

- **Open directly** — double-click `index.html` to open it in your browser, **or**
- **Serve it** (recommended for full audio support):

  ```bash
  # Python 3
  python3 -m http.server 8000
  # then visit http://localhost:8000
  ```

## 🌐 Hosting on GitHub Pages

This repo is ready for free hosting:

1. Go to **Settings → Pages**.
2. Under **Build and deployment**, set **Source** to *Deploy from a branch*.
3. Choose branch **`main`** and folder **`/ (root)`**, then **Save**.
4. After a minute the game is live at `https://pasuay.github.io/thunder-math/`.

## 📚 Built on math-education research

Each mode's progression mirrors established fluency standards and peer-reviewed research in early mathematics education — moving deliberately from concrete → pictorial → abstract. The same list is available in-game via the **"Built on math-education research"** panel on the start screen.

**Add & Subtract** — the strategy ladder (count on → make ten → bridging through ten → doubles → think-addition):

- National Governors Association Center for Best Practices & Council of Chief State School Officers (2010). *Common Core State Standards for Mathematics*, 1.OA.C.6 & 2.OA.B.2. Washington, DC.
- Paliwal, V., & Baroody, A. J. (2020). Fostering the learning of subtraction concepts and the subtraction-as-addition reasoning strategy. *Early Childhood Research Quarterly, 51*, 403–415. [doi:10.1016/j.ecresq.2019.05.008](https://doi.org/10.1016/j.ecresq.2019.05.008)
- Kullberg, A., Björklund, C., Runesson Kempe, U., Brkovic, I., Nord, M., & Maunula, T. (2024). Improvements in learning addition and subtraction when using a structural approach in first grade. *Educational Studies in Mathematics, 117.* [doi:10.1007/s10649-024-10339-z](https://doi.org/10.1007/s10649-024-10339-z)

**Times Tables** — the array (groups) model and structure-first ordering of the tables (anchor facts 2, 5, 10 → the 0× and 1× rules → 3, 4 → the hard cluster 6–9):

- Barmby, P., Harries, T., Higgins, S., & Suggate, J. (2009). The array representation and primary children's understanding and reasoning in multiplication. *Educational Studies in Mathematics, 70*(3), 217–241. [doi:10.1007/s10649-008-9145-1](https://doi.org/10.1007/s10649-008-9145-1)
- Mulligan, J., & Mitchelmore, M. (2009). Awareness of pattern and structure in early mathematical development. *Mathematics Education Research Journal, 21*(2), 33–49. [doi:10.1007/BF03217544](https://doi.org/10.1007/BF03217544)
- NRICH (Pennant, with Way & Askew), University of Cambridge, and NCETM — *Arrays, Multiplication and Division* / *Arrays and Area Models* (practitioner guidance).

**Tens & Ones (Place Value)** — base-ten blocks and the part-whole (number-bond) idea, following concrete → pictorial → abstract:

- Bruner, J. S. (1966). *Toward a Theory of Instruction.* Cambridge, MA: Belknap Press of Harvard University Press — the enactive–iconic–symbolic (concrete–pictorial–abstract) sequence.
- Dienes, Z. P. (1960). *Building Up Mathematics.* London: Hutchinson Educational — base-ten / multi-base blocks that make "a ten is ten ones" visible.

**Number Patterns** — skip-counting and number sequences build the awareness of pattern and structure that underpins multiplication and early algebra:

- Mulligan, J., & Mitchelmore, M. (2009). Awareness of pattern and structure in early mathematical development. *Mathematics Education Research Journal, 21*(2), 33–49. [doi:10.1007/BF03217544](https://doi.org/10.1007/BF03217544)
- Clements, D. H., & Sarama, J. (2009). *Learning and Teaching Early Math: The Learning Trajectories Approach* (1st ed.). New York: Routledge — counting and patterning learning trajectories.

## 🙌 Credits

Developed by **Pashur Au Yeung**.

## 📄 License

Released under the [MIT License](LICENSE) — free to use, modify, and share, with attribution. three.js is also MIT-licensed.
