IndicCode Tutor 🇮🇳
Bridging the coding language barrier for India. Learn to code in your mother tongue.

📋 Table of Contents
Overview

The Problem

The Solution

Key Features

Architecture & Tech Stack

Target Audience

Repository Structure

Future Roadmap

📖 Overview
IndicCode Tutor is an AI-powered educational platform designed to democratize computer science education in India. It helps students learn programming by providing code explanations, real-time error detection, and conceptual tutoring in their native languages.

By leveraging AWS Bedrock and Generative AI, we translate complex logic into culturally relevant examples in Hindi, Telugu, Tamil, Bengali, and Marathi, ensuring that language proficiency never becomes a barrier to technical competency.

🛑 The Problem
Students in Tier-2 and Tier-3 colleges across India often possess strong logical skills but struggle with the English-centric nature of programming documentation and error messages.

Syntax vs. Concept: Beginners often confuse syntax errors with logical gaps.

Language Barrier: Technical jargon in English can be intimidating for regional language speakers.

Context Gap: Standard tutorials use western examples that may not resonate with rural Indian students.

💡 The Solution
IndicCode Tutor acts as a personal AI companion that sits between the student and the code. It doesn't just translate text; it translates context.

Contextual Translation: Converts "Syntax Error: Unexpected Token" into a native language explanation of why the code failed.

Cultural Analogies: Explains complex concepts (like Loops or Recursion) using everyday Indian examples (e.g., counting steps, queuing systems).

Interactive Learning: Allows students to click on any line of code to get a breakdown in their preferred language.

✨ Key Features
1. 🗣️ Multi-Language Support
Full support for Hindi (हिंदी), Telugu (తెలుగు), Tamil (தமிழ்), Bengali (বাংলা), and Marathi (मराठी). The system persists language preferences across sessions.

2. 🧠 AI-Powered Explanations (AWS Bedrock)
Utilizes Claude-3 / Llama-3 via AWS Bedrock to generate beginner-friendly, line-by-line explanations.

Input: A complex Python list comprehension.

Output: A step-by-step breakdown in Telugu explaining the data transformation.

3. 🐞 Intelligent Error Detection
Goes beyond standard compiler messages. The Error_Detector component identifies:

Syntax Errors (with fix suggestions).

Logical Errors (infinite loops, unreachable code).

Best Practice violations (PEP 8).

4. 🖱️ Interactive Code Analysis
Click-to-Explain: Users can click specific lines to understand dependencies.

File Support: Handles .py, .js, and .java files (up to 5MB).

Paste-and-Learn: Direct code pasting for quick debugging.

🏗 Architecture & Tech Stack
The system follows a Serverless, Cloud-Native Architecture optimized for scalability and cost-efficiency.

Code snippet
graph LR
    User[Student] --> Frontend[React + Tailwind]
    Frontend --> API[AWS API Gateway]
    API --> Lambda[AWS Lambda Functions]
    Lambda --> Bedrock[AWS Bedrock AI]
    Lambda --> Trans[Language Translator]
    
    subgraph "Core Logic"
    Lambda
    end
Technology Stack
Layer	Technology	Purpose
Frontend	React.js, Tailwind CSS	Responsive UI, Syntax Highlighting, State Management
API Layer	AWS API Gateway	Managed endpoints, Throttling, Authentication
Compute	AWS Lambda	Serverless execution for Code Analysis & Error Detection
AI Engine	AWS Bedrock	Generative AI for code explanation & cultural adaptation
Infra	AWS CDK	Infrastructure as Code (IaC)
🎯 Target Audience
Tier-3 College Students: Engineering students proficient in regional languages but developing English skills.

Self-Learners: Individuals in rural areas learning via mobile devices.

Bootcamp Students: Beginners needing instant clarification on complex logic without waiting for a human tutor.

📂 Repository Structure
This repository contains the core design and requirement artifacts generated via Kiro and refined by the team.

Bash
IndicCode-Tutor/
├── design.md          # Full Technical Architecture & Component Design
├── requirements.md    # User Stories, Personas, and Acceptance Criteria
├── tasks.md           # Implementation Roadmap & Progress
├── assets/            # Diagrams and Presentation Deck
└── README.md          # Project Documentation
🚀 Future Roadmap
Phase 1 (MVP): Core support for Python/JS in Hindi/Telugu.

Phase 2: Integration of IDE Plugins (VS Code extension).

Phase 3: Voice-to-Text queries for students to ask questions verbally.

Phase 4: Offline mode for areas with low internet connectivity.

Team AXion_Vibers
Participating in AI For Bharat - Student Track





