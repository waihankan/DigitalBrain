---
draft: 
title: AI Agents, Evaluations, and Environments
tags:
  - research-position
---

## Initial Email Description

[Apply here](https://docs.google.com/forms/d/e/1FAIpQLSe5CQVbYeXBRzWAYfUZfeqdGp-7fmsha8s7IrRZ7xRGhDLNFA/viewform?usp=dialog) for research opportunities in AI agents, evaluations, and environments. 

We are interested in **quantifying AI agent capabilities, leveraging metrics to build better agents, and leveraging AI agents to build better evaluations across domains like cybersecurity, software engineering, machine learning, and computer use.**

<u>Prior Work</u>

1. [BountyBench](https://bountybench.github.io/)
2. [CyberGym](http://cybergym.io/)
3. [Cybench](https://cybench.github.io/)

> Cybench has been leveraged by the US/UK AISI for joint pre-deployment tests on [Claude 3.5 Sonnet](https://www.nist.gov/system/files/documents/2024/11/19/Upgraded%20Sonnet-Publication-US.pdf) and [OpenAI o1](https://www.nist.gov/system/files/documents/2024/12/18/US_UK_AI%20Safety%20Institute_%20December_Publication-OpenAIo1.pdf#page8), featured in system cards by Anthropic ([Claude 3.7 Sonnet](https://assets.anthropic.com/m/785e231869ea8b3b/original/claude-3-7-sonnet-system-card.pdf) and [Claude 4](https://www-cdn.anthropic.com/6be99a52cb68eb70eb9572b4cafad13df32ed995.pdf)) and Amazon ([Nova Premier](https://assets.amazon.science/f6/c5/79dceb124593b3356566ad6723af/the-amazon-nova-premier-technical-report-and-model-card.pdf)), incorporated into the [xAI Risk Management Framework](https://x.ai/documents/2025.02.20-RMF-Draft.pdf), used as the only benchmark for [OWASP’s LLM Exploit Generation Whitepaper](https://genai.owasp.org/resource/owasp-llm-exploit-generation-v1-0-pdf/), rated the [10th highest at ICLR out of 11,672 submissions](https://papercopilot.com/statistics/iclr-statistics/iclr-2025-statistics/), and won first prize in the [SafeBench competition](https://www.mlsafety.org/safebench/winners#winners).

As a next step, we are recruiting motivated students to help with the following efforts:

1. **Building evaluations** to quantify agent capabilities. **Design and implement complex, realistic environments** to quantify AI agent capabilities across domains like cybersecurity and software engineering, and computer use. 
    
2. **Developing agents**. LM agents show great promise but still have many limitations when applied to various domains. We developed an initial agent for cybersecurity and are working to improve it, with plans to extend to other domains. **Help us integrate powerful tools, fit scaling laws, fine-tune models, and design multi-agent architectures.**
    
3. **Building environments to run RL on agents**. Create environments with realistic rewards that allow us to run RL on our agents and improve their capabilities across various domains.

We are looking for candidates with **strong programming skills, a passion for building, and rigor and attention to detail.** 

---

## Preparation

![[Pasted image 20250625161130.png]]



## Docker

1. A container is defined by its image as well as any configuration options you provide to it when you create or start it. 
2. When a container is removed, any changes to its state that aren't stored in persistent storage disappear.
3. `docker run -d -p 8080:80 docker/welcome-to-docker
	1. 8080 is the port on host
	2. 80 is the port in the container
4. 


![[Pasted image 20250626140852.png]]`


---

# Preparation for 45 minutes live technical interview / chat

- [ ] data processing and document search, indicating strong backend development skills.
- [ ] conceived the project, built the initial codebase, developed the evaluation tasks, and authored the majority of the research paper
- [ ] **Python-based system** that evaluates language model agents on 40 professional-level Capture the Flag (CTF) cybersecurity challenges.
	- [ ] **Complex evaluation frameworks** with both binary success metrics and fractional subtask scoring
	- [ ] **Automated logging and analysis systems** for large-scale model evaluation
	- [ ] The project spans six cybersecurity categories: **cryptography, web security, reverse engineering, forensics, exploitation, and miscellaneous challenges**. [github +2](https://cybench.github.io/) This suggests their technical interview problems will likely draw from similar domains, testing your ability to **solve complex algorithmic and security-related coding** challenges.
	- [ ] Given that they mentioned "we will just give you a problem and have you fill out some functions," expect **Python programming challenges** that reflect their research focus. Based on their Cybench work and technical background, prepare for:
	- [ ] **Core Programming Skills**:
		- [ ] **Python fundamentals including data structures, algorithms, and object-oriented programming**.
		- [ ] file processing, API integration, and data manipulation
		- [ ] input validation
	- [ ] **AI/ML Evaluation Concepts**
		- [ ] **model evaluation methodologies, benchmarking approaches, and statistical analysis**.
		- [ ] **precision/recall, cross-validation, and performance metrics**
	- [ ] **System Design and Integration**
		- [ ] **API integration, containerization concepts, and multi-component system design**.



### Modular, Scalable Code

- [ ] **Process and analyze structured data** (JSON, CSV) (Evaluation pipelines)
- [ ] **Implement scoring and evaluation functions** reflecting their benchmarking work
- [ ] **Handle API responses and error management** given their multi-model evaluation system




---

1. [[Python Foundation]]
2. [[Files, Inputs and Outputs, Argparse in Python]]
3. [[Pandas CSV Files]]
4. [[Writing Tests in Python]]
5.  [[String Processing]] 
6. [[Logging, Error Handling]]
7. [[API ]]

8. ML Concepts, Evaluation, Read Paper
9. Prepare insightful questions
10. Interview Prep

---
### **Interview Day Strategy**

#### **45-Minute Interview Structure** (Likely breakdown):

- **5 min**: Introductions and problem explanation
- **30 min**: Coding problem (2-3 functions)
- **5 min**: Code review and optimization discussion
- **5 min**: Questions and wrap-up
-

#### **Problem-Solving Framework**:

1. **Clarify requirements** (2-3 min):
    - Ask about input/output formats
    - Confirm edge cases
    - Understand evaluation criteria
2. **Plan approach** (3-5 min):
    - Outline solution strategy
    - Identify key data structures
    - Consider error handling
3. **Implement solution** (15-20 min):
    - Start with core logic
    - Add error handling
    - Write clean, readable code
4. **Test and refine** (5-7 min):
    - Walk through examples
    - Check edge cases
    - Optimize if time permits


**Technical Setup**:

- [ ]  IDE/editor configured and tested
- [ ]  Python environment ready
- [ ]  Basic libraries imported (json, csv, requests, etc.)
- [ ]  Clean workspace, good lighting
- [ ]  Backup plan (alternative editor/environment)

**Mental Preparation**:

- [ ]  Review their Cybench paper abstract (5 min)
- [ ]  Practice explaining your approach clearly
- [ ]  Prepare 2-3 technical questions about their work
- [ ]  Get good sleep and arrive early



1. ML Evaluation Concepts
2. Prepare Technical Questions on Past Work
3. Prepare General Interview Questions; Review my Resume, Challenge, Team Work (gossamer)
4. Setup Check
5. Mindset for Clean Code, Modular, Scalable, make a checklist

---

# Conversation Regarding the Role

1. Previous Experience
2. Challenge from past experience
3. Weakness and Strength
4. 


--- 

- **Personal motivation and passion for research**
    
- **Technical storytelling from past projects**
    
- **Teamwork and communication examples**
    
- **Curiosity about AI evaluation and safety**
    
- **Alignment with their mission and tools**
































