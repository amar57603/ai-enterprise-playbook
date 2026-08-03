# 🤖 AI Enterprise Playbook

Welcome to the **AI Enterprise Playbook**! This repository contains a strict, battle-tested set of rules designed to tame autonomous AI coding agents (like Gemini, Claude, Cursor, or ChatGPT) and force them to build software like senior enterprise engineers.

If you are tired of AI agents hallucinating features, writing generic "AI-looking" Tailwind UIs, skipping security protocols, or turning your git history into a chaotic mess, this playbook is for you.

## 🌟 What is this?

By default, AI agents try to guess what you want and rush straight into writing code. This leads to spaghetti code, security holes, and frustrating rewrites. 

This playbook enforces a **Strict 6-Phase Workflow** and **8 Core Rulesets** that force the AI to:
1. **Stop and Interview You:** The AI is forbidden from coding until it conducts a 10-question Discovery interview and generates a formal Product Requirements Document (PRD).
2. **Design Beautiful UIs:** The AI is forbidden from using default, generic "AI aesthetics." It must focus on premium designs (glassmorphism, smooth animations, overlapping grids).
3. **Respect Security Levels:** You dictate the Security Level (1, 2, or 3) in the PRD, and the AI adapts its security checks and pentesting accordingly.
4. **Follow Git Discipline:** The AI is forced to use conventional commits and logical branching.

---

## ⚡ Quick Install (1-Line Command)
If you are using Cursor, Windsurf, or PearAI, you can download the entire compiled playbook straight into your project with a single command. Run this in your terminal at the root of your project:

**Mac / Linux:**
```bash
curl -sSL https://raw.githubusercontent.com/amar57603/ai-enterprise-playbook-a/main/compiled/.cursorrules -o .cursorrules
```

**Windows (PowerShell):**
```powershell
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/amar57603/ai-enterprise-playbook-a/main/compiled/.cursorrules" -OutFile ".cursorrules"
```

*(For ChatGPT, Claude, Copilot, or Aider, scroll down to the "How to Install" section at the bottom!)*

---

## 📂 The 8 Core Rulesets

Inside the `rules/` folder, you will find the 8 pillars of this playbook:

1. `project_workflow_rules.md`: The backbone. Enforces the 6-phase pipeline (Discovery → Architecture → Security → Design → Build → Review). Includes the mandatory PRD generation and Pre-Release Pentest.
2. `design_direction_rules.md`: The "Anti-AI-Generica" rule. Forces the AI to provide premium, highly-customized design options and mandates that the *Human* makes all final aesthetic decisions.
3. `architecture_style_rules.md`: Rules for folder structure, state management, and separation of concerns.
4. `backend_rules.md`: Strict database, API, and backend security protocols (scaled by Security Level).
5. `frontend_rules.md`: Component standards, prop typing, and client-side validation rules.
6. `accessibility_performance_rules.md`: Web Vitals, screen reader compatibility, and optimization standards.
7. `git_discipline_rules.md`: Strict conventional commit requirements and branch management.
8. `agent_rules.md`: Meta-rules for how the AI must behave (no guessing, no rushing, mandatory approvals).

## 🚀 How to Use This

You can inject these rules into your favorite AI coding environment by copying the markdown files into your agent's system prompt, cursor rules (`.cursorrules`), or global workspace configurations.

### 1. The PRD Interview
When you start a new project with these rules active, the AI will immediately halt and say:
`"[Current Phase: Phase 1 - Discovery]"`

It will then ask you 10 critical questions:
*   Scope, Audience, Features, Inspiration, Timeline, User Flows, Scale, Content, Out-of-Scope, and Security Level.

### 2. The PRD Generation
The AI will generate a `PRD.md` file. This acts as the unbreakable contract. If it's not in the PRD, the AI is not allowed to build it without asking you first!

### 3. The Build Phase
The AI will execute the architecture, security, and design phases, always stopping to ask for your explicit approval before moving to the next stage.

### 4. The Pentest
If you set the project to **Security Level 3**, the AI will perform a mandatory pre-release pentest, scanning for vulnerabilities (SQLi, Auth gaps, Rate Limiting) before you are allowed to deploy.

## 🧠 How to Install the Playbook into your AI's "Brain"

Because this playbook is split into 8 separate, highly detailed files, you cannot just drag and drop the folder into a chat window. To force the AI to strictly obey these rules automatically, you need to compile them into the AI's "brain". 

### Method 1: AI IDEs (Cursor, Windsurf, PearAI)
These editors look for a single `.cursorrules` or `.windsurfrules` file in the root of your project.

**Mac/Linux:**
```bash
cat path/to/ai-enterprise-playbook/rules/*.md > .cursorrules
```
**Windows (PowerShell):**
```powershell
Get-Content path\to\ai-enterprise-playbook\rules\*.md -Raw | Set-Content .cursorrules
```

### Method 2: VS Code Extensions (GitHub Copilot, Roo Code, Continue.dev)
If you are using Copilot Workspace or VS Code extensions like Roo/Cline, they rely on workspace instructions.
1. Compile the rules into a single file using the commands from Method 1.
2. Rename the compiled file to `.github/copilot-instructions.md` (for Copilot) or `.clinerules` / `.roomodes` (for Roo/Cline).
3. The extension will automatically read and enforce these rules in your workspace.

### Method 3: Web-Based AI (ChatGPT Pro, Claude Projects, Lovable, v0)
1. Compile the 8 files into a single text file on your computer.
2. Open **ChatGPT** -> Go to "Customize ChatGPT" -> Paste the text into the "Instructions" box.
3. Open **Claude** -> Create a new "Project" -> Upload the compiled `.md` file into "Project Knowledge".
4. Open **Lovable / v0** -> Paste the rules into your initial project prompt or global system instructions.

### Method 4: CLI Autonomous Agents (Aider, Devin, OpenDevin)
For terminal-based coding agents like Aider, they look for convention files.
- Compile the files into a single `CONVENTIONS.md` file in the root of your directory.
- Start your agent (e.g., `aider`) and give it one strict initial prompt: *"Read CONVENTIONS.md and strictly follow it for all tasks."*

### Method 5: Gemini / Antigravity Global Skills (Advanced)
If you are running a local terminal-based agent framework (like Google Antigravity), you want these rules to be global so they apply to all projects. You can set up a Sync Script that takes the local files and injects them straight into the agent's global configuration directory.

**Windows PowerShell Sync Script Example:**
```powershell
# 1. Grab all the rules
$content = "---`r`nname: enterprise-playbook`r`ndescription: CRITICAL RULE: You MUST use this skill for EVERY task.`r`n---`r`n`r`n" + (Get-Content path\to\rules\*.md -Raw)

# 2. Add the strict initialization trigger
$newHeader = "<CRITICAL_SYSTEM_DIRECTIVE>`r`nYou are equipped with the Global Enterprise Playbook Plugin... `r`n`r`nYour first message must start with this exact phrase: `r`n`"[Current Phase: Phase 1 - Discovery] `r`nGlobal Enterprise Playbook Plugin Active.`"`r`n</CRITICAL_SYSTEM_DIRECTIVE>`r`n`r`n"
$content = $content -replace "(?sm)(---.*?---`r`n)", "`$1$newHeader"

# 3. Inject it straight into the AI's global brain config
Set-Content -Path C:\Path\To\Agent\Config\skills\enterprise_playbook\SKILL.md -Value $content
```

## 🤝 Contributing
Feel free to fork this repository, add your own workflow rules, and submit a PR! Let's build a world where AI agents write clean, secure, and beautiful code by default.
