# 🚀 Senior Laravel Partner — Claude Skill

> **Turn Claude into a senior multi-disciplinary Laravel partner who follows *your* codebase — not textbook best practices.**

A Claude skill for every Laravel developer. Drop it into Claude, and it acts as a **Senior Engineer, Lead Architect, DB Expert, QA Lead, and Product Owner** — all at once. It reads your existing code first, follows your conventions, and never over-engineers.

## 👨‍💻 Author

**Syed Anwar Ahmed Shah**
- LinkedIn: [Syed-Anwar-Ahmed-Shah](https://pk.linkedin.com/in/syed-anwar-ahmed-shah-2aa16a190)
- GitHub: [Syed-Anwar-Ahmed-Shah](https://github.com/anwaar12345)
---

## 📖 Table of Contents

- [Why This Skill?](#-why-this-skill)
- [What It Does](#-what-it-does)
- [The 5 Roles It Plays](#-the-5-roles-it-plays)
- [What It Won't Do](#-what-it-wont-do-by-design)
- [Installation Guide](#-installation-guide)
- [How to Use (Step-by-Step for Beginners)](#-how-to-use-step-by-step-for-beginners)
- [Quick Demo](#-quick-demo)
- [Common Problems & Solutions](#-common-problems--solutions)
- [Summary Table](#-summary-table)
- [Requirements](#️-requirements)
- [Reference Files](#-reference-files)
- [Customization](#-customization)
- [Author](#-author)
- [Contributing](#-contributing)
- [License](#-license)

---

## ✨ Why This Skill?

Most AI coding tools give you "best practice" code that doesn't match your project. This skill does the opposite:

- 🧠 **Reads your codebase first** — extracts your patterns (controller, service, model, blade, JS) and clones them.
- 📋 **Plan-first, code later** — always shares a point-wise plan and waits for your approval before writing code.
- 🔍 **Version-aware** — detects your PHP/Laravel version from `composer.json` and uses only compatible syntax.
- 🚫 **No over-engineering** — no random repositories, interfaces, DTOs, events, or tests you didn't ask for.
- 🛡️ **Edge cases, not tests** — handles null, not-found, unauthorized, duplicates, concurrency — in code, not in test files.
- 💼 **Job/Queue when it matters** — uses jobs for slow tasks, external APIs, retries, and fire-and-forget work — not blindly.
- 📖 **Teaches you** — explains end-to-end flows in simple words, without missing a single technical detail.

---

## 🎯 What It Does

When you give it a task like:

> *"Add a search filter to the users listing"*

It will:

1. **Read** your existing codebase — find similar features, extract the pattern.
2. **Detect** your PHP/Laravel version from `composer.json`.
3. **Share a plan** — point-wise, with files, edge cases, assumptions, and scope.
4. **Wait for your approval.**
5. **Execute** — following your exact conventions.
6. **Report** — with status, pattern used, and edge cases covered.

No code before approval. No surprise abstractions. No "improvements" you didn't ask for.

---

## 🧩 The 5 Roles It Plays

| Role | What It Does |
|---|---|
| **Senior Laravel Engineer** | Production-grade code, strict types, security-first |
| **Lead Architect** | Respects your existing boundaries, doesn't invent new layers |
| **DB Expert** | Schema design, query optimization, migration safety |
| **QA Lead** | Covers every edge case — without writing tests you didn't ask for |
| **Product Owner** | Understands the real task, prevents scope creep |

---

## 🚫 What It Won't Do (By Design)

- ❌ Add Repository/Interface/DTO patterns when your project doesn't use them
- ❌ Write tests when you didn't ask
- ❌ "Improve" adjacent code that wasn't in the task
- ❌ Use PHP 8.3 syntax on a PHP 8.1 project
- ❌ Jump into code without a plan and approval
- ❌ Create jobs/events when a simple service method is enough

---

## 📦 Installation Guide

This section covers **complete installation** — from start to finish. Every step is detailed so there's no confusion.

---

### 🎯 Installation Overview

| Step | What to Do | Time |
|---|---|---|
| 1 | Create a Claude account | 2 minutes |
| 2 | Enable Code Execution | 1 minute |
| 3 | Download the repo | 1 minute |
| 4 | Extract the folder | 1 minute |
| 5 | Re-zip the folder | 1 minute |
| 6 | Upload to Claude | 2 minutes |
| 7 | Turn the skill ON | 30 seconds |
| 8 | Test it | 1 minute |

**Total: ~10 minutes**

---

### Step 1: Create a Claude Account

If you already have a Claude account, skip to **Step 2**.

1. Go to **[https://claude.ai](https://claude.ai)**
2. Click the **"Sign up"** button.
3. Sign up using either:
   - **Google account** (easiest)
   - **Email** (you'll need to verify)
4. Fill in your name and details.
5. Your account is ready.

> 💡 **The Free plan is enough** — no paid plan needed to use this skill.

---

### Step 2: Enable Code Execution (Required)

This setting is **mandatory** for the skill to work:

1. Log in to Claude.ai.
2. Click your **profile icon** (name or avatar) in the left sidebar.
3. Select **"Settings"** from the dropdown.
4. Open the **"Capabilities"** tab.
5. Find **"Code execution and file creation"**.
6. **Toggle it ON** (it should turn blue/green).

> ⚠️ **If this is OFF, the skill will upload but won't work.**

---

### Step 3: Download the Repo

1. Go to this GitHub repo: `https://github.com/anwaar12345/laravel-skill-partner`
2. Click the green **"Code"** button (top right of the page).
3. Select **"Download ZIP"** from the dropdown.
4. The ZIP file will download to your computer:
   - **Windows:** `Downloads` folder
   - **macOS:** `Downloads` folder
   - **Linux:** `~/Downloads`

**The file will be named something like:**
```text
laravel-skill-partner.zip