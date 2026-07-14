# Bar-Ilan University — Algorithms 1 (89-220)
## Interactive Exam Practice Simulator for Antigravity

This repository contains a custom agentic **Skill** designed for the **Antigravity IDE** to run interactive, realistic exam practice simulations. The system is custom-tailored to mimic the style, grading severity, and curriculum guidelines of the Bar-Ilan University Algorithms 1 course.

---

## 🌟 Key Features
- **10 Core Curriculum Topics**: Covers all 10 modules from Polynomials & FFT to Linear Programming & Smallest Enclosing Discs.
- **Past Exam Vault**: Pre-loaded with official exams:
  - `Past_Exams/` directory containing Moed A, B, C papers from 2024–2026.
- **English Course Summaries**: Pre-loaded in `Summaries/` for reference.
- **Structured Practice Sessions**: Automatically generates clean directory files for questions, user templates, and detailed solutions.

---

## ⚙️ How to Install in Antigravity

Since Antigravity automatically detects workspace skills, installing this is as simple as placing the `.agents` folder in your workspace root:

### Step 1: Clone or Copy the Repository
Copy the `.agents` folder into the root of your active project workspace:
Your workspace folder should look like this:
```
BIU_Algo1_study_skills/
├── .agents/
│   └── skills/
│       └── biu-algo1-exam-practice/
│           ├── SKILL.md
│           └── references/
│               ├── Past_Exams/
│               └── Summaries/
├── Past_Exams/
├── Summaries/
├── Practice_Sessions/
└── README.md
```

### Step 2: Trigger the Exam Practice
Open the workspace in your Antigravity IDE. Once loaded, open a chat session with your AI Coding Assistant and say:
> *"Let's start the exam simulation"* or *"Test me using the biu-algo1-exam-practice skill."*

---

## 📂 How to Open and Preview Files

### 💜 Option 1: Open as an Obsidian Vault (Recommended)
This repository is pre-configured with Obsidian settings (including a `.obsidian/` directory) to render math formulas, tables, and links perfectly.
1. Open **Obsidian**.
2. Click **Open folder as vault**.
3. Select the root folder of this repository (`BIU_Algo1_study_skills`).
4. You can write your answers directly in Obsidian, and LaTeX math formulas (e.g. $O(|V|^\omega)$) will render instantly.

### 🌐 Option 2: Preview in Antigravity IDE
1. Open the repository root folder in the **Antigravity IDE**.
2. Use the **Markdown Preview** feature built into the IDE to read the question sheets and solutions.
3. Click the absolute `file:///` links output by the agent in the chat to open files directly in your editor workspace.

