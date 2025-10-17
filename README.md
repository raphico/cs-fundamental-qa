# CS-Fundamentals-Q&A: Systems Engineer's Active Recall Bank

This repository serves as the personalized Question Bank for mastering core Computer Science and Distributed Systems concepts.

## Purpose and Methodology

This repository's sole purpose is to facilitate retrieval practice (testing the memory) for deep, non-superficial understanding.

This repo contains:

- High-Leverage Questions: Questions are focused on application, trade-offs, mechanisms, and failure scenarios, not rote definitions.
- Structured Learning: Content is organized to align with foundational texts like CS:APP, TCP/IP illustrated, OSTEP, and DDIA

Every question in this repository was generated after applying the Feynman Technique (explaining the concept simply) and identifying knowledge gaps.

## How to Use This Repo (The AI Quizzing Protocol)

This protocol is used to transform the static questions into an interactive, spaced repetition test using a Large Language Model (LLM).

1. Preparation

   - Select Questions: Based on the planned review schedule (1-day, 3-day, 1-week intervals), select a mixed batch of questions from different folders (CSAPP, Concurrency, etc.).
   - Set the Scene: Open the LLM (e.g., Gemini, ChatGPT) and paste the Quizzing Prompt below.

2. The Quizzing Prompt (MANDATORY)

   ```
   "Act as an extremely knowledgeable but non-spoiling Technical Interviewer for a [x] role. I will paste the next question now. Do not provide the answer. I will provide my complete answer first. Your only response should be one of the following:
   1. 'CORRECT. Proceed to the next question.'
   2. 'ALMOST. I noticed a slight factual error regarding [subtle concept, e.g., thread vs. process]. Try refining your explanation.'
   3. 'CONCEPTUAL GAP. Your answer is missing the core principle of [specific area, e.g., encoding variability]. What is the primary reason for using a log?'
   ```

3. Execution
   - Answer from Memory: Answer the questions in the chat, relying solely on your brain.
   - Close the Loop: If the AI indicates an error or gap, immediately stop the test and return to the original textbook/source material to correct that knowledge point before proceeding.

This process prioritizes deep knowledge retrieval over simply getting the answer correct

## Explanation of Columns

| Column            | Full Name / Purpose        | Explanation                                                                                                                                                                                                    |
| :---------------- | :------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **ID**            | Unique Question Identifier | Used for quick referencing and tracking the question.                                                                                                                                                          |
| **Question**      | The Retrieval Prompt       | This is the core of the exercise. Questions are deliberately designed to test your mechanisms ("How does it work?") and applications ("Why is this relevant?"). This column forces active recall.              |
| **Status**        | Completion Status          | Tracks the current state of your mastery for this question: **PENDING** (Not yet reviewed), **REVIEW** (Answered incorrectly, needs spaced repetition soon), **MASTERED** (Answered correctly multiple times). |
| **Last Reviewed** | Date of Last Attempt       | Used for implementing _Spaced Repetition_. This date tells you when you last successfully retrieved the answer, informing when you should next be quizzed on it (e.g., 3 days later, 1 week later).            |
