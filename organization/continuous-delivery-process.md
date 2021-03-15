---
description: >-
  This page describes how development teams collaborate on code of Promethist
  Platform components using Git SCM and YouTrack issue tracker.
---

# Platform Continuous Delivery Process

## YouTrack links

* Kanban board - [https://promethist.myjetbrains.com/youtrack/agiles/119-15/current](https://promethist.myjetbrains.com/youtrack/agiles/119-15/current)
* Unstaged unresolved issues - [unfurl:Issues: Stage: {No stage} organization: Platform \#Unresolved](https://promethist.myjetbrains.com/youtrack/issues?q=Stage%3A%20%7BNo%20stage%7D%20organization%3A%20Platform%20%23Unresolved)
* The most voted list - [unfurl:Issues: order by: votes desc ](https://promethist.myjetbrains.com/youtrack/issues?q=order%20by%3A%20votes%20desc%20)

## Platform issues and their stages

### A\) Out of delivery

Such issues can be created by **any user of the Flowstorm platform** and can have any of the following _non-delivery_ states, which can be changed only by representatives of the Platform team:

* **Submitted** \(initial state of created issue\)
* **Won't fix** \(terminal state of issue\)
* **Duplicate** \(terminal state of issue\)
* **To be discussed** \(needs more information/specification\)
* **Can't reproduce** \(temporary state - needs more information, otherwise we won't fix it\)

Relevant issues are moved into Backlog as the initial stage _before delivery_ and change its state to **Open** automatically. The platform team prioritizes issues when it's moving them forward from stage to stage and giving them work precedence inside every delivery stage applying the following set of rules:

1. Bugs first.
2. Issues with more votes first \(except bugs having priority Critical or Show-stopper\).
3. Higher priority takes precedence \(priority value is always confirmed by Team, not issue Reporter\).

#### Issue priority

In general, only bugs can have priority Critical or Show-stopper. Other types of issues should use only priorities Major, Normal \(default\), Minor.

### B\) Staged into delivery

Issues can be staged into delivery only by **members of the Platform team** \(pavel.ducho@promethist.ai currently does that for issues created by non-members of the Platform team\). They can have one of the following _delivery_ states:

* **Open**
* **In Progress**
* **On Hold**
* **Fixed** \(terminal state of issue\)

related to predefined following _delivery_ stages:

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">Development</th>
      <th style="text-align:left">Review</th>
      <th style="text-align:left">Done</th>
      <th style="text-align:left">Done</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><em>Description</em>
      </td>
      <td style="text-align:left">Work on the issue</td>
      <td style="text-align:left">Code review and user testing performed upon Merge Request</td>
      <td style="text-align:left">Code automatically deployed into the <b>preview</b> environment after change
        request merged</td>
      <td style="text-align:left">Code deployed into the <b>production</b> environment</td>
    </tr>
    <tr>
      <td style="text-align:left"><em>Responsible role</em>
      </td>
      <td style="text-align:left"><b>Developer</b>
      </td>
      <td style="text-align:left"><b>Tech Reviewer<br />UX Reviewer</b>
      </td>
      <td style="text-align:left"><b>-</b>
      </td>
      <td style="text-align:left"><b>Senior member of TECH squad</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><em>Relevant state(s)</em>
      </td>
      <td style="text-align:left">
        <ul>
          <li><b>Open</b> (initial, set automatically by moving from Backlog)</li>
          <li><b>In Progress</b> (set by developer)</li>
          <li><b>On Hold</b> (can&apos;t continue, reaction to comment needed)</li>
        </ul>
      </td>
      <td style="text-align:left">
        <ul>
          <li><b>In Progress</b>
          </li>
          <li><b>On Hold</b> (can&apos;t continue, reaction/fix needed)</li>
        </ul>
      </td>
      <td style="text-align:left">
        <ul>
          <li><b>Fixed</b>
          </li>
        </ul>
      </td>
      <td style="text-align:left">
        <ul>
          <li><b>Fixed</b>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><em>Git reference</em>
      </td>
      <td style="text-align:left"><code>APP-X-description</code> branch</td>
      <td style="text-align:left">Merge request from branch <code>APP-X-description</code> to <code>master</code>
      </td>
      <td style="text-align:left"><code>master</code> branch</td>
      <td style="text-align:left"><code>tag</code> on <code>master</code> branch</td>
    </tr>
    <tr>
      <td style="text-align:left"><em>Endpoint</em>
      </td>
      <td style="text-align:left">Developer&apos;s <code>localhost</code>
      </td>
      <td style="text-align:left"><code>app-X.flowstorm.ai</code>
      </td>
      <td style="text-align:left"><code>app-preview.flowstorm.ai</code>
      </td>
      <td style="text-align:left"><code>app.flowstorm.ai</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><em>Database</em>
      </td>
      <td style="text-align:left">develop</td>
      <td style="text-align:left">develop</td>
      <td style="text-align:left">production</td>
      <td style="text-align:left">production</td>
    </tr>
    <tr>
      <td style="text-align:left"><em>Namespace</em>
      </td>
      <td style="text-align:left">-</td>
      <td style="text-align:left">X</td>
      <td style="text-align:left">preview</td>
      <td style="text-align:left">default</td>
    </tr>
  </tbody>
</table>

## Changes related to Core project

If platform change request \(APP-X issue\) requires to make any changes in Core project then it will go following way

1. Developer will create branch `APP-X-description` on core project to make request changes
2. Developer will make these changes available as `X-SNAPSHOT` version by creating Pull Request \(PR\) in Github core project which will be then processed by Gitlab CI pipeline as changes will be mirrored to Flowstorm Gitlab repository _Note: this PR should contain comment informing that this PR was requested by flowstorm project, to prevent it's merge before flowstorm MR was tested_
3. Developer will set X-SNAPSHOT as `flowstorm.core.version` in platform `settings.xml` file
4. Developer will create platform merge request to make issue ready for testing on domain `app-X.flowstorm.ai`
5. If testing identifies any problem, developer will push update commits to core and/or platform APP-X-description branch \(when there was only update of core X-SNAPSHOT, he can re-run flowstorm MR pipeline to update app-X.flowstorm.ai\)
6. If core PR and flowstorm MR are both okay from reviewer + tester's point of view then

   1. Core PR will be merged and new Core version will be released by tagging master branch to `X.Y`
   2. Developer or Flowstorm reviewer will change `flowstorm.core.version` in platform `settings.xml` file to `X.Y` 
   3. Flowstorm MR will be merged 

 

## Changes related to NLP backend services

When platform change is requesting update of NLP backend services \(illusionist, duckling, triton\), such update can be currently done by pushing update to `master` branch of particular NLP service project which will result into preview deployment on URL `https://<nlp-service>.preview.flowstorm.ai`. Flowstorm change request deployment should be updated to reference preview instance of NLP service\(s\) by adding `<nlp-service>.url` properties to `app.properties` configuration in `deploy/admin/templates/configmap.yaml` file. As soon as updated versions of NLP services are tested and released to production, specific `<nlp-service>.url` properties should be removed from associated PR.

