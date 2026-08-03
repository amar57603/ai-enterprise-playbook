<div align="center">

# 🤖 AI Enterprise Playbook

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)
[![AI Agents](https://img.shields.io/badge/Supported-All_AI_Agents-blue.svg)](#)
[![Cursor](https://img.shields.io/badge/Cursor-Ready-black.svg)](#)

**A battle-tested set of strict rules designed to tame autonomous AI coding agents (Cursor, Copilot, Aider, Antigravity) and force them to build software like senior enterprise engineers.**

[**Installation**](#-how-to-install-choose-your-weapon) • [**How it Works**](#-how-it-works) • [**The 8 Pillars**](#-whats-under-the-hood-the-8-pillars)

---

</div>

## 🎯 The Problem

By default, AI agents try to guess what you want and rush straight into writing code. This leads to:
- 🍝 **Spaghetti Code:** No architecture or planning.
- 🎨 **Generic Designs:** Default "AI-looking" Tailwind interfaces.
- 🔓 **Security Holes:** Skipping basic auth and database protections.
- 🗑️ **Messy Git History:** Meaningless commit messages and chaotic branching.

> **The Solution:** This playbook injects a strict **6-Phase Workflow** into your AI's brain, forcing it to stop, interview you, plan the architecture, and ask for permission before writing a single line of code.

---

## 🚀 How to Install (Choose your weapon)

You only need to install this once per project. Choose the method that matches your AI tool:

### 1️⃣ The Zero-Touch Install *(For Autonomous Agents)*
If you use an agent like **Antigravity, Aider, or Cline**, just paste this prompt into your chat window:
```text
Please download the Enterprise Playbook from https://raw.githubusercontent.com/amar57603/ai-enterprise-playbook/main/compiled/SKILL.md and install it as a persistent rule/skill in your system.
```

### 2️⃣ The 1-Line Install *(For Cursor, Windsurf, PearAI)*
Run this command in your project's terminal to instantly configure your AI IDE:

**🍎 Mac / Linux:**
```bash
curl -sSL https://raw.githubusercontent.com/amar57603/ai-enterprise-playbook/main/compiled/.cursorrules -o .cursorrules
```
**🪟 Windows (PowerShell):**
```powershell
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/amar57603/ai-enterprise-playbook/main/compiled/.cursorrules" -OutFile ".cursorrules"
```

<details>
<summary><b>🌍 Click here for the Universal Method (ChatGPT, Copilot, Llama, OpenDevin, etc.)</b></summary>

Because this playbook compiles down to a single raw Markdown text file, **it works on literally any AI model in existence.** It doesn't matter if you are using Claude, ChatGPT, a local Llama 3 model, Codex, or OpenDevin. 

* **GitHub Copilot / Roo Code:** Download the compiled file from Method 2 and rename it to `.github/copilot-instructions.md` (Copilot) or `.clinerules` (Roo).
* **Web Interfaces (ChatGPT / Claude):** Open your AI interface, go to "Custom Instructions" or "Project Knowledge", and paste the raw text.
* **Local Models & Custom Agents (Llama, OpenDevin, Codex):** Just download `SKILL.md` from the `compiled/` folder and feed it directly into your agent's system prompt or starting context window. 
* **Design Agents (Lovable / v0.dev):** Paste the raw text into your initial project prompt.
</details>

---

## ⚙️ How it Works

Once installed, your AI is strictly bound by a new workflow. It will no longer rush into coding. Instead, it will guide you through these phases:

| Phase | What the AI must do |
| :--- | :--- |
| 🕵️‍♂️ **1. Discovery** | Halts immediately and asks you a 10-question interview. Generates a formal `PRD.md` (Product Requirements Document). |
| 🏗️ **2. Architecture** | Plans folder structure, state management, and dependencies before coding. |
| 🔒 **3. Security** | Reads the "Security Level" from your PRD and enforces backend protections. |
| ✨ **4. Design** | Avoids generic UI components. Focuses on premium aesthetics (glassmorphism, animations). |
| 💻 **5. Build** | Writes code using strict conventional commits and Git discipline. |
| 🛡️ **6. Pentest** | If Security Level 3 is active, the AI conducts a pre-release security scan (SQLi, Auth gaps) before deployment. |

---

## 📂 What's Under the Hood? (The 8 Pillars)

If you prefer to read or edit the rules yourself, look inside the `rules/` folder. The playbook is powered by 8 core pillars that enforce enterprise-grade standards across every domain:

| Rule File | Description & Capabilities |
| :--- | :--- |
| 🧠 `agent_rules.md` | **AI Behavioral Constraints.** Forces the AI to be honest, requires strict self-verification, and mandates **Multi-Agent Delegation** (spawning subagents to avoid context overload). Also forces the AI to proactively recommend missing skills and IDE extensions. |
| 📋 `project_workflow_rules.md` | **The 6-Phase Pipeline.** Enforces the Discovery interview, PRD generation, and the mandatory Pre-Release Pentest. |
| 🎨 `design_direction_rules.md` | **The Anti-AI-Generica Rule.** Forces the AI to provide premium, custom design options (glassmorphism, overlapping grids) and mandates that the Human makes all final aesthetic decisions. |
| 🗄️ `backend_rules.md` | **Security & Databases.** Strict API standards, ORM rules, and database security protocols (scaled by Security Level). |
| 🖥️ `frontend_rules.md` | **UI Components.** Component boundaries, prop typing, state management, and client-side validation rules. |
| 🧱 `architecture_style_rules.md` | **Clean Code.** Rules for folder structure, dependency injection, and separation of concerns. |
| ⚡ `accessibility_performance_rules.md` | **Web Vitals.** Screen reader compatibility, contrast ratios, and optimization standards. |
| 🌿 `git_discipline_rules.md` | **Version Control.** Strict conventional commit requirements and branch management logic. |

---

<div align="center">
<b>Built for developers who want AI speed without sacrificing Enterprise quality.</b><br><br>

Feel free to fork this repository, tweak the rules for your own team, and submit a PR! Let's build a world where AI agents write clean, secure, and beautiful code by default.
</div>
