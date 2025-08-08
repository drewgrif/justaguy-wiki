---
title: Homelab AI Chat
layout: default
---

# 🧠 Chat

## 🗂️ AI Setup Timeline on TrueNAS SCALE (LLaMA + Open WebUI + Cloud APIs)

### 🖥️ **1. System Overview**
- Running on **TrueNAS SCALE**
- Hardware:
  - 🧠 **AMD Ryzen 5 PRO 4650G (6C/12T)**
  - 💾 **32GB RAM**
  - 🧩 **GIGABYTE B550I AORUS PRO AX motherboard**
- No discrete GPU (using integrated Vega graphics)
- AI services hosted via app containers on TrueNAS


### 🌐 **3. Install Open WebUI**
- **Open WebUI** is a sleek browser-based interface for chatting with AI models.
- It connects directly to your Ollama backend and offers:
  - ✅ Multi-user support (great for sharing with family)
  - 🕓 Chat history and per-user settings
  - 🔀 Easy model switching between local and API models
- Hosted directly on the **TrueNAS SCALE** box.

**Note:**  
I was able to add family members using group permissions, and I limited which models each could access — great for keeping things simple and budget-conscious.


### ✅ Chosen Cloud Models (Based on Cost-Effectiveness):

| Model                       | Why I Chose It                           |
|----------------------------|------------------------------------------|
| `anthropic/claude-3.7-sonnet` | Great quality, lower cost than GPT-4     |
| `gpt-4o-mini`              | Smart & responsive at a good price       |
| `anthropic/claude-3.5-haiku` | Super cheap and fast for general tasks   |
| `o1-mini`                  | Lightweight experimental model           |
| `gpt-3.5-turbo`            | Cheap fallback with decent capability    |
