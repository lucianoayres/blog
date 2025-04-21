---
title: "Build AI Agents on Ubuntu with CrewAI in 5 Minutes Without Breaking Your System Python"
date: 2025-04-21 00:00:00
author: "Luciano Ayres"
---

Although CrewAI’s docs claim Ubuntu’s default Python 3.10 works, it can fail in practice. 

In this tutorial you’ll be safely guided through using pyenv and a dedicated virtual environment to install and run CrewAI without touching your system Python.

---

## Why Ubuntu’s Default Python Breaks CrewAI and How Pyenv Fixes It

[CrewAI](https://www.crewai.com/) is a fantastic agentic framework, but on Ubuntu its default Python (3.10.x) can trip you up—especially since updating the system interpreter isn’t an option without risking your OS tools. **Although the official CrewAI docs state it works on Python 3.10** ([see installation guide](https://docs.crewai.com/installation#text-tutorial)), in my experience it broke when running on 3.10 due to missing `typing.Self`. We’ll use **pyenv** to install Python 3.12.10, then set up CrewAI in a clean virtual environment.

### 🛠️ Prerequisites

- Ubuntu 22.04 (or similar)  
- `curl`, `git`, and basic build tools installed.

---

## 1. Install Pyenv

Run the installer:

```bash
curl https://pyenv.run | bash
```

Add these lines **exactly** to the end of your `~/.bashrc`:
Run these commands to append the necessary setup lines to your `~/.bashrc`. This directly adds the exports and init call for you:

```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' \
    >> ~/.bashrc
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' \
    >> ~/.bashrc
echo 'eval "$(pyenv init - bash)"' \
    >> ~/.bashrc
```

Reload your shell so these take effect:

```bash
exec "$SHELL"
```

Verify:

```bash
pyenv --version
```

> **Need extra help?**  
> – See the official Pyenv installation guide:  
>   https://github.com/pyenv/pyenv#installation

---

## 2. Install the `uv` Tool

`uv` makes installing Python‑based CLIs super fast and will be used later to install the CrewAI command‑line interface:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

You’ll now have the `uv` command available.

---

## 3. Install Python 3.12.10 with Pyenv

CrewAI works best on ≥ 3.12:

```bash
pyenv install 3.12.10
pyenv shell 3.12.10
python --version   # → Python 3.12.10
```

---

## 4. Create & Activate a Virtual Environment

Keep everything isolated in your project folder (e.g. `~/projects/my_project`):

```bash
mkdir -p ~/projects/my_project
cd ~/projects/my_project

python -m venv .venv
source .venv/bin/activate
```

Your prompt should now start with `(.venv)`.

---

## 5. Install CrewAI

With the venv active:

```bash
uv tool install crewai
```

Confirm the install:

```bash
crewai --version
# e.g. crewai, version 0.114.0
```

---

## 6. Create a New “Crew”

Scaffold an agent project—here we’ll call it `my_project`:

```bash
crewai create crew my_project
```

When prompted:

1. **Select provider**: choose **3** for Gemini  
2. **Select model**: choose **1** for `gemini/gemini-1.5-flash`  
3. **Enter API key**: get a free key at  
   https://aistudio.google.com/app/apikey

CrewAI will generate this structure:

```
my_project/
├── .gitignore
├── knowledge/
├── pyproject.toml
├── README.md
├── .env
└── src/
    └── my_project/
        ├── __init__.py
        ├── main.py
        ├── crew.py
        ├── tools/
        │   ├── custom_tool.py
        │   └── __init__.py
        └── config/
            ├── agents.yaml
            └── tasks.yaml
```

---

## 7. Run Your Crew

Switch into your project and start it:

```bash
cd my_project
crewai run
```

You should see something like:

```
─── Crew Execution Started ───
Name: crew
ID: abc123…

→ Task: xyz789  Status: In Progress
…
─── Crew Completion ───
```

🎉 **Congrats!** You’ve just run your first CrewAI project on Ubuntu—without touching the system Python.

---

### 🔧 Cleanup Tip

If you ever need to start fresh:

```bash
deactivate
rm -rf .venv my_project
```

---

Feel free to adapt folder names or swap in any Python 3.12.x patch as needed.
