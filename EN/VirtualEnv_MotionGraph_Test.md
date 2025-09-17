# Technical Verification of Virtual Game Environment and Automated Bot Control (Short Report)

## Overview (2025.06.05)

This document records the results of building a local network game environment and testing basic operations using automated control bots (OpenKore).  
Future applications are envisioned in the field of video production (Adobe After Effects) and as a foundation for AI-driven dynamic control.

---

## 🧭 Objectives

- Experimental setup of a client/Bot control system within a local virtual environment  
- Initial verification of a control pipeline concept that can be linked with video production tools (After Effects)  
- Use as a technical record, portfolio material, and self-study log  

---

## ⚙️ Environment

| Category          | Technology / Tool Used               |
|-------------------|--------------------------------------|
| Server            | `rAthena` (MariaDB 10.5.29)          |
| Client            | `FroggoClient.exe` (XKore compatible)|
| Bot Control       | `OpenKore` (XKore0 mode)             |
| Development Tools | VSCode / Python / Visual Studio      |
| Data Management   | HeidiSQL                             |
| AE Control        | JSON → JSX (ExtendScript)            |
| OS                | Windows 11 (local execution)         |

---

## 🚧 Results After One Week

### ✅ Server / Client Environment

- Built local server using `rAthena` and confirmed MariaDB connection  
- Successfully launched client (`FroggoClient.exe`)  
- Verified coordination between Bots and client over local network  

### ✅ Bot (OpenKore) Verification

- Launched 4 Bots, confirmed login and automated actions (movement, follow, combat, item pickup)  
- Tested simple macro scripts (e.g., NPC job change handling)  
- Verified client integration via XKore0  
- Plugin development not yet started (future task)  

---

## 🔍 Limitations and Pending Work

- Autonomous AI behaviors (e.g., hunting ground selection, item selling, storage handling) not implemented  
- Perl plugin structure analysis and development not yet started  
- Concept of converting game logs into JSON for AE integration still under consideration  
- Current log usage limited to simple notifications via Discord integration  

---

## 🎬 Relationship with After Effects Control

- Developing an environment for AE layer automation via Python → JSON → JSX  
- Currently implementing external extraction of AE layer structures  
- Integration with video production AI is suspended for now (data input confirmed)  
- Full-scale AE linkage will be organized in a separate phase  

---

## ✍️ Policy on Usage and Disclosure

- This project is strictly a **technical verification in a local environment with a non-public game setting**.  
  No commercial use or public network deployment is intended.  
- Open-source software used (rAthena, OpenKore) complies with their respective licenses.  
- Planned to be published on GitHub and personal websites as technical portfolio / experimental log (actual game title not disclosed).  

---

## 🔭 Future Outlook

- Convert Bot logs and action histories into JSON for application in AE automation and motion graphics generation  
- Trial introduction of AI integration via OpenKore plugins and state machine control  
- Utilize as a **bi-directional control interface** between virtual environments and media editing tools  
- Consider lightweight tool development as a personal rendering engine / AE control UI  

---

## 🗂 Related Tags

`#OpenKore` `#rAthena` `#Python` `#AfterEffects` `#JSX` `#JSON` `#BotAutomation`  
`#VideoProduction` `#LocalServerSetup` `#TechPortfolio` `#AIIntegration`

---
