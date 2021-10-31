---
description: >-
  This page describes how development teams collaborate on code of Promethist
  Platform components using Git SCM and YouTrack issue tracker.
---

# Platform Continuous Delivery Process

## YouTrack links

* Kanban board - [https://promethist.myjetbrains.com/youtrack/agiles/119-15/current](https://promethist.myjetbrains.com/youtrack/agiles/119-15/current)
* Unstaged unresolved issues - [unfurl:Issues: Stage: {No stage} organization: Platform #Unresolved](https://promethist.myjetbrains.com/youtrack/issues?q=Stage%3A%20%7BNo%20stage%7D%20organization%3A%20Platform%20%23Unresolved)
* The most voted list - [unfurl:Issues: order by: votes desc ](https://promethist.myjetbrains.com/youtrack/issues?q=order%20by%3A%20votes%20desc)

## Platform issues and their stages

### A) Out of delivery

Such issues can be created by **any user of the Flowstorm platform** and can have any of the following _non-delivery_ states, which can be changed only by representatives of the Platform team:

* **Submitted** (initial state of created issue)
* **Won't fix** (terminal state of issue)
* **Duplicate** (terminal state of issue)
* **To be discussed** (needs more information/specification)
* **Can't reproduce** (temporary state - needs more information, otherwise we won't fix it)

Relevant issues are moved into Backlog as the initial stage _before delivery_ and change its state to **Open** automatically. The platform team prioritizes issues when it's moving them forward from stage to stage and giving them work precedence inside every delivery stage applying the following set of rules:

1. Bugs first.
2. Issues with more votes first (except bugs having priority Critical or Show-stopper).
3. Higher priority takes precedence (priority value is always confirmed by Team, not issue Reporter).

#### Issue priority

In general, only bugs can have priority Critical or Show-stopper. Other types of issues should use only priorities Major, Normal (default), Minor.

### B) Staged into delivery

Issues can be staged into delivery only by **members of the Platform team** (pavel.ducho@promethist.ai currently does that for issues created by non-members of the Platform team). They can have one of the following _delivery_ states:

* **Open**
* **In Progress**
* **On Hold**
* **Fixed** (terminal state of issue)

related to predefined following _delivery_ stages:

|                     | Development                                                                                                                                                                                                                       | Review                                                                                                                                                                                  | Done                                                                                     | Done                                              |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- | ------------------------------------------------- |
| _Description_       | Work on the issue                                                                                                                                                                                                                 | Code review and user testing performed upon Merge Request                                                                                                                               | Code automatically deployed into the **preview** environment after change request merged | Code deployed into the **production** environment |
| _Responsible role_  | **Developer**                                                                                                                                                                                                                     | <p><strong>Tech Reviewer</strong><br><strong>UX Reviewer</strong></p>                                                                                                                   | **-**                                                                                    | **Senior member of TECH squad**                   |
| _Relevant state(s)_ | <ul><li><strong>Open</strong> (initial, set automatically by moving from Backlog)</li><li><strong>In Progress</strong> (set by developer)</li><li><strong>On Hold</strong> (can't continue, reaction to comment needed)</li></ul> | <ul><li><strong>In Progress</strong></li><li><strong>On Hold</strong> = can't continue, reaction/update needed (see comments)</li><li><strong>Fixed</strong> = UX review done</li></ul> | <ul><li><strong>Fixed</strong></li></ul>                                                 | <ul><li><strong>Fixed</strong></li></ul>          |
| _Git reference_     | `APP-X-description` branch                                                                                                                                                                                                        | Merge request from branch `APP-X-description` to `master`                                                                                                                               | `master` branch                                                                          | `tag` on `master` branch                          |
| _Endpoint_          | Developer's `localhost`                                                                                                                                                                                                           | `app-X.flowstorm.ai`                                                                                                                                                                    | `app-preview.flowstorm.ai`                                                               | `app.flowstorm.ai`                                |
| _Database_          | develop                                                                                                                                                                                                                           | develop                                                                                                                                                                                 | production                                                                               | production                                        |
| _Namespace_         | -                                                                                                                                                                                                                                 | X                                                                                                                                                                                       | preview                                                                                  | default                                           |

## Changes related to Core project

If platform change request (APP-X issue) requires to make any changes in Core project then it will go following way

1. Developer will create branch `APP-X-description` on core project to make request changes
2. Developer will make these changes available as `X-SNAPSHOT` version by creating Pull Request (PR) in Github core project which will be then processed by Gitlab CI pipeline as changes will be mirrored to Flowstorm Gitlab repository\
   _Note: this PR should contain comment informing that this PR was requested by flowstorm project, to prevent it's merge before flowstorm MR was tested_
3. Developer will set X-SNAPSHOT as `flowstorm.core.version` in platform `settings.xml` file
4. Developer will create platform merge request to make issue ready for testing on domain `app-X.flowstorm.ai`
5. If testing identifies any problem, developer will push update commits to core and/or platform APP-X-description branch (when there was only update of core X-SNAPSHOT, he can re-run flowstorm MR pipeline to update app-X.flowstorm.ai)
6.  If core PR and flowstorm MR are both okay from reviewer + tester's point of view then

    1. Core PR will be merged and new Core version will be released by tagging master branch to `X.Y`
    2. Developer or Flowstorm reviewer will change `flowstorm.core.version` in platform `settings.xml` file to `X.Y`&#x20;
    3. Flowstorm MR will be merged&#x20;

    &#x20;

## Changes related to NLP backend services

When platform change is requesting update of NLP backend services (illusionist, duckling, triton), such update can be currently done by pushing update to `master` branch of particular NLP service project which will result into preview deployment on URL `https://<nlp-service>.preview.flowstorm.ai`. Flowstorm change request deployment should be updated to reference preview instance of NLP service(s) by adding `<nlp-service>.url` properties to `app.properties` configuration in `deploy/admin/templates/configmap.yaml` file. As soon as updated versions of NLP services are tested and released to production, specific `<nlp-service>.url` properties should be removed from associated PR.
