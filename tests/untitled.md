# Untitled



|  | Development | Review | Testing | Done |
| :--- | :--- | :--- | :--- | :--- |
| Description |  | Code review performed via Merge Request | User testing performed in preview environment | Deployment |
| Responsible role | **Developer** | **Reviewer** \(senior member of Platform Development squad\) | **Tester** \(member of Platform UX squad\) | Senior member of Platform team |
| Relevant state\(s\) | **Open** \(initial, set automatically by move from Backlog\) **In Progress** \(set by developer\) **On Hold** \(can't continue, reaction to comment needed\) | **In Progress** | **In Progress** **On Hold** \(can't continue, reaction/fix needed\) | **In Progress** \(after moving from Testing stage\) **Fixed** \(terminal state of issue, after deployment performed\) |
| Gitlab reference | `PROJ-X-description` branch | Merge request `PROJ-X-description` to `master` | `master` branch | `tag` on `master` branch |
| Runtime environment | Developer's local develop + develop databases \(dependencies to locally running, preview or production services\) | Reviewer's local develop \(if needed\) | [preview](https://preview.promethist.app/) | [production](https://promethist.app/) |

