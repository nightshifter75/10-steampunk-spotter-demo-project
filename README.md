# ⚙️ Steampunk Spotter Demo Project

Welcome to the **Steampunk Spotter Demo Project** repository! 🚀

This repository serves as a testing ground and playground for testing and validating **Ansible Playbooks** generated with the assistance of various **AI models** (Gemini ♊, ChatGPT 🤖, and Claude 🧠).

---

## 🎯 Purpose & Scope

The main goal is to leverage AI-assisted code generation to automate network and infrastructure workflows, while using **Steampunk Spotter** to analyze, scan, and optimize the generated Ansible playbooks for quality, security, and best practices.

### 🧪 Current Test Focus:
1. **🌐 Cisco IOS Operations**: Automating configuration management, routine maintenance, and operational tasks on Cisco network devices.
2. **🛡️ Commvault Windows Patching**: Streamlining and automating patching operations for Windows Servers running Commvault backup infrastructure.

---

## 🤖 AI Models In Use

To compare playbooks, optimization suggestions, and code quality, playbooks are generated across multiple AI engines:

| AI Engine | Cisco IOS Playbooks | Commvault Windows Playbooks |
| :--- | :---: | :---: |
| **Google Gemini** | 🟢 Active | 🟢 Active |
| **OpenAI ChatGPT** | 🟢 Active | 🟢 Active |
| **Anthropic Claude** | 🟢 Active | 🟢 Active |

---

## 🔍 Validation with Steampunk Spotter

All generated playbooks undergo scanning using **Steampunk Spotter** (`spotter scan`) to ensure:
* ⚠️ Detection of deprecated modules and parameters.
* 🛡️ Identification of security vulnerabilities and misconfigurations.
* ⚡ Best practice recommendations and quality enhancements.

---

## 📂 Repository Structure

```text
.
├── spotterlogin.sh          # Helper script for Spotter authentication
├── cisco-ios/               # Playbooks for Cisco IOS automation
├── commvault-windows/       # Playbooks for Commvault patching automation
└── README.md
