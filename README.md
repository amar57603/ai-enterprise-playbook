# 🤖 AI Enterprise Playbook

A battle-tested set of strict rules designed to tame autonomous AI coding agents (Cursor, Copilot, Aider, Antigravity) and force them to build software like senior enterprise engineers.

---

## 🎯 The Problem

By default, AI agents try to guess what you want and rush straight into writing code. This leads to:
- **Spaghetti Code:** No architecture or planning.
- **Generic Designs:** Default "AI-looking" Tailwind interfaces.
- **Security Holes:** Skipping basic auth and database protections.
- **Messy Git History:** Meaningless commit messages and chaotic branching.

**The Solution:** This playbook injects a strict **6-Phase Workflow** into your AI's brain, forcing it to stop, interview you, plan the architecture, and ask for permission before writing a single line of code.

---

## 🚀 How to Install (Choose your weapon)

You only need to install this once per project. Choose the method that matches your AI tool:

### 1️⃣ The Zero-Touch Install (For Autonomous Agents)
If you use an agent like **Antigravity, Aider, or Cline**, just paste this prompt into your chat window:
> *"Please download the Enterprise Playbook from `https://raw.githubusercontent.com/amar57603/ai-enterprise-playbook-a/main/compiled/SKILL.md` and install it as a persistent rule/skill in your system."*

### 2️⃣ The 1-Line Install (For Cursor, Windsurf, PearAI)
Run this command in your project's terminal to instantly configure your AI IDE:

**Mac / Linux:**
```bash
curl -sSL https://raw.githubusercontent.com/amar57603/ai-enterprise-playbook-a/main/compiled/.cursorrules -o .cursorrules
```
**Windows (PowerShell):**
```powershell
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/amar57603/ai-enterprise-playbook-a/main/compiled/.cursorrules" -OutFile ".cursorrules"
```

<details>
<summary><b>🛠️ Click here for Other AI Tools (ChatGPT, Copilot, Claude, v0)</b></summary>

* **GitHub Copilot / Roo Code:** Download the compiled file from Method 2 and rename it to `.github/copilot-instructions.md` (Copilot) or `.clinerules` (Roo).
* **ChatGPT Pro / Claude:** Download the compiled file, open your AI web interface, and paste the contents into the "Custom Instructions" or "Project Knowledge" section.
* **Lovable / v0.dev:** Paste the raw contents into your initial project prompt.
</details>

---

## ⚙️ How it Works

Once installed, your AI is strictly bound by a new workflow. It will no longer rush into coding. Instead, it will guide you through these phases:

| Phase | What the AI does |
| :--- | :--- |
| **1. Discovery** | Halts immediately and asks you a 10-question interview. Generates a formal `PRD.md` (Product Requirements Document). |
| **2. Architecture** | Plans folder structure, state management, and dependencies before coding. |
| **3. Security** | Reads the "Security Level" from your PRD and enforces backend protections. |
| **4. Design** | Avoids generic UI components. Focuses on premium aesthetics (glassmorphism, animations). |
| **5. Build** | Writes code using strict conventional commits and Git discipline. |
| **6. Pentest** | If Security Level 3 is active, the AI conducts a pre-release security scan (SQLi, Auth gaps) before deployment. |

---

## 📂 What's Under the Hood?

If you prefer to read or edit the rules yourself, look inside the `rules/` folder in this repository. The playbook is powered by 8 core pillars:
1. `project_workflow_rules.md` (The 6-phase pipeline)
2. `design_direction_rules.md` (The "Anti-AI-Generica" UI rules)
3. `architecture_style_rules.md` (Clean code standards)
4. `backend_rules.md` (Database and API security)
5. `frontend_rules.md` (Component standards)
6. `accessibility_performance_rules.md` (Web Vitals)
7. `git_discipline_rules.md` (Branching and commits)
8. `agent_rules.md` (AI behavioral constraints)

## 🤝 Contributing
Feel free to fork this repository, tweak the rules for your own team, and submit a PR! Let's build a world where AI agents write clean, secure, and beautiful code by default.
