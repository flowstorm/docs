---
description: >-
  This page describes how development teams collaborate on code of Promethist
  Platform components using Git SCM and YouTrack issue tracker.
---

# Continuous Delivery Process

## YouTrack links

* Kanban board - [https://promethist.myjetbrains.com/youtrack/agiles/119-15/current](https://promethist.myjetbrains.com/youtrack/agiles/119-15/current)
* Unstaged unresolved issues - [unfurl:Issues: Stage: {No stage} organization: Platform \#Unresolved](https://promethist.myjetbrains.com/youtrack/issues?q=Stage%3A%20%7BNo%20stage%7D%20organization%3A%20Platform%20%23Unresolved)
* The most voted list - [unfurl:Issues: order by: votes desc ](https://promethist.myjetbrains.com/youtrack/issues?q=order%20by%3A%20votes%20desc%20)

## Issues and their stages

### A. Out of delivery

Such issues can be created by **any user of the Platform** and can have any of following _non-delivery_ states, which can be changed only by representatives of Platform team:

* **Submitted** \(initial state of created issue\)
* **Won't fix** \(terminal state of issue\)
* **Duplicate** \(terminal state of issue\)
* **To be discussed** \(needs more information/specification\)
* **Can't reproduce** \(temporary state - needs more information, otherwise we won't fix it\)

Relevant issues are moved into Backlog as the initial stage _before delivery_ and change its state to **Open** automatically. Platform team prioritises issues when it's moving them forward from stage to stage and giving them work precedense inside every delivery stage applying following set of rules

1. Bugs first
2. Issues with more votes first \(except bugs having priority Critical or Show-stopper\)
3. Higher priority takes precedense \(priority value is always confirmed by Team, not issue Reporter\)

#### Issue priority

In general, only bugs can have priority Critical or Show-stopper. Other types of issues should use only priorities Major, Normal \(default\), Minor.

### B.  Staged into delivery

Issues can be staged into delivery only be **members of Platform team** \(pavel.ducho@promethist.ai currently does that for issues created by non-members of Platform team\). They can have one of following _delivery_ states

* **Open**
* **In Progress**
* **On Hold**
* **Fixed** \(terminal state of issue\)

related to predefined following _delivery_ stages:

|  | Development | Review | Testing | Done |
| :--- | :--- | :--- | :--- | :--- |
| Description |  | Code review performed via Merge Request | User testing performed in preview environment | Deployment |
| Responsible role | **Developer** | **Reviewer** \(senior member of Platform Development squad\) | **Tester** \(member of Platform UX squad\) | Senior member of Platform team |
| Relevant state\(s\) | **Open** \(initial, set automatically by move from Backlog\) **In Progress** \(set by developer\) **On Hold** \(can't continue, reaction to comment needed\) | **In Progress** | **In Progress** **On Hold** \(can't continue, reaction/fix needed\) | **In Progress** \(after moving from Testing stage\) **Fixed** \(terminal state of issue, after deployment performed\) |
| Git reference | `PROJ-X-description` branch | Merge request `PROJ-X-description` to `master` | `master` branch | `tag` on `master` branch |
| Runtime environment | Developer's local develop + develop databases \(dependencies to locally running, preview or production services\) | Reviewer's local develop \(if needed\) | [preview](https://preview.promethist.app) | [production](https://promethist.app) |

