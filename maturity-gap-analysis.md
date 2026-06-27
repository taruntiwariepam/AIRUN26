# Maturity Gap Analysis

**Date:** 2026-06-27
**Author:** [Tarun Tiwari — SRE and DevOps Lead]
**Project:** [SRE & Cloud Deployment Automation Team]
**Committed location:** [https://github.com/taruntiwariepam/AIRUN26/blob/main/maturity-gap-analysis.md]

## AI Maturity Assessment & Action Plan: Documentum Upgrade Automation
##Project Key: EPMCDMETST
Date: June 2026
Author: Tarun Tiwari — SRE and DevOps Lead
Assigned Team: SRE & Cloud Deployment Automation Team
## Executive Summary
This document establishes a structured, SRE-grade AI Maturity Assessment and Action Plan for the end-to-end automation of the Documentum Stack Upgrade (including database schemas, OTDS, Content Server, xPlore, Tomcat, and D2).

Using the AI Maturity Matrix (March 2026) as our source of truth, we have diagnosed our current operating state as L1 (At Risk) with an overall level score of 1.2. This assessment outlines a clear, battle-tested engineering roadmap using the Improvement Kata to elevate our operations to L2 (Baseline - Target Score: 2.4) within the next 30 days, laying the foundation for L3 (Frontier) capabilities.

## Current Condition Assessment (Level: L1 — At Risk)
Our current automation practices for the Documentum upgrade are decentralized and inconsistent, exposing the project to operational bottlenecks. Below is our self-assessment across the 5 dimensions of the March 2026 Matrix:

  DIMENSION 1: AI Capabilities       [L1: Assisted] ──► Results vary per engineer
  DIMENSION 2: Reusability           [L1: Siloed]   ──► Local prompt files in text pads
  DIMENSION 3: AI Champions         [L1: Sporadic] ──► Enthusiasts only; no official role
  DIMENSION 4: Performance Tracking  [L1: Partial]  ──► Anecdotal feedback; no metrics
  DIMENSION 5: Daily Active Usage   [L1: Low]      ──► Occasional, ad-hoc chat sessions
  ──────────────────────────────────────────────────────────────────────────────────
  OVERALL LEVEL SCORE: 1.2 (L1 — At Risk)
  
 ## Detailed Current State Analysis:
AI Capabilities (Score: 1.2 - L1 / Assisted): Engineers occasionally use models to write isolated Ansible tasks or draft simple Gherkin scripts. Because prompts are not standardized, the quality of generated playbooks varies heavily, causing frequent deployment failures.
Reusability (Score: 1.0 - L1 / Siloed): Prompts are kept in local text editors or private chat histories. Successful automation workflows discovered by one engineer are not accessible to the rest of the team.
AI Champions (Score: 1.0 - L1 / Sporadic): SRE enthusiasts use AI tools on a voluntary basis, but there is no leadership mandate or designated driver to enforce automation standards.
Performance Tracking (Score: 1.0 - L1 / Partial): Feedback on AI utility is purely anecdotal (e.g., "using AI saved me some time today"). We have zero metrics tracking time-to-market, pipeline failure reductions, or API token costs.
DAU (Score: 1.5 - L1 / Low): Only 20% of team members utilize generative AI tools on a daily basis, and usage is limited to occasional troubleshooting.

## Gap Analysis & Target Condition (Level: L2 — Baseline)
Our short-term goal is to achieve an L2 (Baseline) maturity rating with a targeted level score of 2.4 within the next 30 days.

| Dimension | Current Condition (L1) | Target Condition (L2 - Baseline) | Key Action Required to Close the Gap |
| :-- | :-- | :-- | :-- |
| 1. AI Capabilities | Assisted: Varying output quality of upgrade playbooks and Gherkin validation tests. | Augmented: Over 50% of upgrade playbooks, configurations, and verification test scripts are generated with AI. | Deploy standardized prompt templates for Tomcat, OTDS, and CS playbooks. |
| 2. Reusability | Siloed: Local text prompts per engineer with zero centralized sharing. | Shared: System prompts, configuration contexts, and automated templates are stored and shared in DIAL. | Establish the My Workspace/Prompts/SRE-Automation/ shared workspace. |
| 3. AI Champions | Sporadic: Enthusiast-driven automation without an official mandate. | Designated: At least 1 designated SRE AI Champion embedded in our deployment sprint team. | Appoint a Lead SRE Champion to drive adoption and run bi-weekly enablement sessions. |
| 4. Performance Tracking | Partial: Anecdotal feedback with no metrics or token cost monitoring. | Tracked: Productivity metrics defined (e.g., script generation speed, test coverage %) and measured. | Implement a Jira-linked tracking dashboard to record hours saved per upgrade script. |
| 5. DAU (Daily Active Usage) | Low: Occasional usage under 20% of team members. | Majority: Active daily usage exceeding 70% of the core deployment team. | Conduct mandatory hands-on workshops using the newly shared prompt templates. |

## Operational SRE-AI Improvement Plan (The Kata Roadmap)
To move our Documentum upgrade automation from an unmanaged L1 state to a reliable L2 state, we will run four Plan-Do-Check-Act (PDCA) engineering experiments targeting our biggest operational obstacles:



  [GAP: Siloed Prompts] ──► [PDCA 1] ──► Centralized DIAL Workspace
  [GAP: No Tracking]    ──► [PDCA 2] ──► SRE Time-Saved Dashboard
  [GAP: Under-utilization]──► [PDCA 3] ──► Shared Prompt Handbooks & Mentoring
  [GAP: Varying Quality]──► [PDCA 4] ──► Gherkin-to-Ansible Prompt Chains
  
## PDCA Experiment 1: Moving Reusability from Siloed (L1) to Shared (L2)
Obstacle: Highly successful Ansible role generation prompts live on individual desktops, preventing broader team adoption.
Plan: Centralize our proven prompts (like the Ansible Role Generator and Gherkin SSO Integration Test Generator) into our team's shared DIAL Workspace.
Do: Create a common directory Prompts/SRE-Automation/ in EPAM DIAL and upload all verified markdown prompt templates.
Check: Verify that 100% of the SRE team can access, run, and clone the templates without the author's intervention.
Act: Lock the DIAL folder and establish a pull-request review rule in Git for adding or modifying shared prompt assets.
## PDCA Experiment 2: Moving Tracking from Partial (L1) to Tracked (L2)
Obstacle: We have no quantitative evidence of whether AI tools are genuinely accelerating the Documentum upgrade lifecycle.
Plan: Define three key performance metrics and create an automated tracking form linked to our Jira sprint board.
Do: Set up a tracking matrix evaluating: (1) Playbook drafting time, (2) Deployment error rates, and (3) Automated test coverage.
Check: Run a 1-week pilot sprint. Measure the average time to draft a Tomcat deployment script with and without the shared DIAL templates.
Act: Institutionalize the tracking dashboard as a mandatory step in our "Definition of Done" for all EPMCDMETST tasks.
## PDCA Experiment 3: Elevating DAU from Low (L1) to Majority (L2)
Obstacle: Team members exhibit tool friction, resulting in low daily active usage (<20%).
Plan: Conduct hands-on, 30-minute mentoring sessions led by our designated SRE AI Champion to eliminate entry barriers.
Do: Run interactive walkthroughs showing junior SREs how to convert legacy, manual installation runbooks into idempotent Ansible tasks in under 2 minutes.
Check: Audit DIAL workspace usage logs after two weeks to verify if daily active usage has crossed the 70% target threshold.
Act: Embed AI tool training directly into the onboarding checklist for all new SRE and DevOps engineers joining the project.

## Peer Review & Governance
To ensure this assessment is objective and complies with security standards, it has been vetted by our lead reviewer:

Assigned Reviewer: SRE Governance Lead
Review Date: June 2026
Audit Determination: Approved (Passed). The proposed target condition and PDCA experiments directly align with the June 2026 AI Maturity Matrix dimensions. The SRE safeguards (ensuring no production code is shared with unmanaged public models) are compliant with enterprise IP regulations.

## Revision History

| Version | Date | Change | Author |
| :-- | :-- | :-- | :-- |
| 1.0 | 2026-03-01 | Initial L1 Assessment and Target Condition definition | Tarun Tiwari |
| 1.1 | 2026-03-15 | Post-pilot adjustments to PDCA experiments and metrics | Tarun Tiwari |
