# n8n-workflow-starforge
<div align="center">
  
# 🤖 Conversational AI Voice Agent

*An intelligent, low-latency voice assistant built on **n8n** that handles phone calls, processes natural language, and books appointments autonomously.*

[![n8n](https://img.shields.io/badge/n8n-Workflow_Automation-EA4B71?style=for-the-badge&logo=n8n)](https://n8n.io/)
[![Twilio](https://img.shields.io/badge/Twilio-Telephony-F22F46?style=for-the-badge&logo=twilio)](https://twilio.com/)
[![Groq](https://img.shields.io/badge/Groq-Llama_3_70B-F55036?style=for-the-badge)](https://groq.com/)
[![Rime AI](https://img.shields.io/badge/Rime_AI-TTS-000000?style=for-the-badge)](https://rime.ai/)
[![Cloudinary](https://img.shields.io/badge/Cloudinary-Audio_Storage-3448C5?style=for-the-badge&logo=cloudinary)](https://cloudinary.com/)

---
</div>

## 📖 Overview

This project implements a fully automated, conversational voice assistant using **n8n**. By connecting **Twilio** for telephony with **Groq's Llama-3-70B** model, the system can hold natural, real-time conversations over the phone. 

What makes this agent powerful is its ability to take action: during a call, it can intelligently trigger tools to **create Google Calendar events** and **append data to Google Sheets** (e.g., booking appointments). The agent's text responses are synthesized into lifelike speech using **Rime AI**, hosted instantly on **Cloudinary**, and played back to the caller seamlessly.

## 📸 Workflow Architecture

> **Note:** This is the visual representation of our n8n automation flow.

*(Delete this text and drag your image here)*

## ⚙️ The Tech Stack

- **[n8n](https://n8n.io/):** The core orchestration engine linking all APIs and services.
- **[Twilio](https://www.twilio.com/):** Receives incoming phone calls and sends the voice/text data to our webhook.
- **[Groq (Llama-3-70B)](https://groq.com/):** Serves as the brain of the agent. It processes the caller's intent, generates conversational text, and decides when to use external tools.
- **[Rime AI](https://rime.ai/):** Converts the LLM's text responses into ultra-realistic, low-latency audio.
- **[Cloudinary](https://cloudinary.com/):** Temporarily stores the generated audio files to generate a direct playback URL.
- **[Google Workspace](https://workspace.google.com/):** Used as memory and action endpoints for the AI to book calendar events and manage sheets.

## 🔄 System Flowchart

Here is the step-by-step execution path of our workflow:

```mermaid
graph TD
    %% Styling
    classDef twilio fill:#F22F46,stroke:#fff,stroke-width:2px,color:#fff;
    classDef n8n fill:#EA4B71,stroke:#fff,stroke-width:2px,color:#fff;
    classDef ai fill:#F55036,stroke:#fff,stroke-width:2px,color:#fff;
    classDef rime fill:#000000,stroke:#fff,stroke-width:2px,color:#fff;
    classDef cloudinary fill:#3448C5,stroke:#fff,stroke-width:2px,color:#fff;
    classDef tools fill:#0F9D58,stroke:#fff,stroke-width:2px,color:#fff;

    %% Nodes
    User(("📱 Caller")):::twilio
    Webhook["⚡ n8n Webhook<br>(Receives Call Data)"]:::n8n
    Agent{"🤖 Groq AI Agent<br>(Llama-3-70B)"}:::ai
    Tools_Cal["📅 Google Calendar<br>(Create Event)"]:::tools
    Tools_Sheet["📊 Google Sheets<br>(Append Appointment)"]:::tools
    TTS["🗣️ Rime AI<br>(HTTP: Text-to-Speech)"]:::rime
    Storage["☁️ Cloudinary<br>(HTTP: Store Audio)"]:::cloudinary
    Response["📤 Webhook Response<br>(Sends Audio URL)"]:::n8n

    %% Flow
    User -->|Initiates Call via Twilio| Webhook
    Webhook --> Agent
    
    Agent -.->|Tool Call| Tools_Cal
    Agent -.->|Tool Call| Tools_Sheet
    
    Agent -->|Generates Text Response| TTS
    TTS -->|Returns Audio File| Storage
    Storage -->|Generates Public URL| Response
    Response -->|Plays Audio via Twilio| User
