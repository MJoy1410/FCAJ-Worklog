---
title: "Self-Assessment"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

During the **First Cloud AI Journey (FCAJ)** program, I studied AWS by topic, applied what I learned to **Tindy**, and took part in community activities. The report documents three main outcomes: my [AWS Cloud Practitioner Essentials worklog](/1-worklog/), the [Tindy proposal and architecture](/2-proposal/), and my [Education talk at FCAJ Community Day](/4-eventparticipated/4.5-event5/) on October 31, 2026.

### Assessment by criterion

| No. | Criterion | Basis for the assessment | Good | Fair | Average |
| --- | --- | --- | --- | --- | --- |
| 1 | **Professional knowledge and skills** | I applied AWS concepts to Tindy's CloudFormation, ECS, RDS, and PDF processing design. The infrastructure repository includes configuration for monitoring ALB health and HTTP 5xx errors. | ✅ |  |  |
| 2 | **Ability to learn** | I organized AWS Cloud Practitioner Essentials topics in my weekly log. When I encountered a new service, I related it to a project scenario, such as using CloudWatch to investigate HTTP 5xx errors. | ✅ |  |  |
| 3 | **Initiative** | I sought out and summarized technical articles on Kiro, AgentCore, and AWS Emulator. I also prepared a talk on getting started with AWS for FCAJ Community Day. | ✅ |  |  |
| 4 | **Responsibility** | While preparing the backend release, I added backup and migration evidence requirements for schema changes. I also added a workflow for the team to rehearse a rollback. | ✅ |  |  |
| 5 | **Discipline** | I organized learning by week and used sprints to track project milestones. I want to keep updates more regular when learning, integration, and reporting overlap. |  | ✅ |  |
| 6 | **Willingness to improve** | I record topics that need further investigation. I want to ask for feedback earlier on each deliverable and record what I changed after each review. |  | ✅ |  |
| 7 | **Communication** | I presented New to AWS? Where Should You Start? in the Education session at FCAJ Community Day. I organized the talk around beginner needs and explained the purpose of three learning resources. | ✅ |  |  |
| 8 | **Teamwork** | Tindy merged backend and frontend changes through pull requests alongside related infrastructure updates. I want to communicate earlier when my changes affect another part of the API, data model, or rollout sequence. |  | ✅ |  |
| 9 | **Professional conduct** | I credited technical sources and used sample documents when describing demo scenarios. I can improve handoffs by briefly recording decisions, open items, and owners. |  | ✅ |  |
| 10 | **Problem-solving** | For API failures, I can trace ALB health, ECS events, and application logs. I need more controlled failure exercises to compare symptoms, test hypotheses, and practice recovery. |  | ✅ |  |
| 11 | **Contribution to the project and community** | My recorded contributions include an ECS backend release workflow, ALB monitoring, alerts for the PDF scanning dead-letter queue, and the Education talk at FCAJ Community Day. | ✅ |  |  |
| 12 | **Overall** | I assess my self-directed learning, application to Tindy, and knowledge sharing positively. My next priorities are stronger operational verification and earlier coordination across the project. | ✅ |  |  |

### Areas for improvement

1. **Record evidence at each release milestone.** For a Tindy deployment or rollback rehearsal, I want to record the version, environment, verification steps, expected result, and observed result together. For the API, I can capture readiness results after the release and after restoring the previous version.
2. **Practice diagnosing and recovering from controlled failures.** In a test environment, I can choose a scenario such as an unhealthy ALB target or repeated delivery of a PDF scan message. Each exercise should include a hypothesis, signals to check, and a way to verify recovery.
3. **Share shorter updates earlier.** When an issue affects another part of the project, I want to report what is complete, who may be affected, and which decision needs help. At the end of each sprint, I can record which feedback I addressed and which items should carry over.
