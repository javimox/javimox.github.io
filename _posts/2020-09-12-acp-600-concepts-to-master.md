---
title:  "Preparing ACP-600 Project Administration in Jira Server: Part I Concepts to master"
toc_sticky: false
categories:
  - sysadmin
tags:
  - certification
  - jira
date: 2020-09-12T21:06:59+02:00
last_modified_at: 2020-09-12T22:16:59+02:00
---

Here you can find what helped me to pass the exam **ACP-600 Project Administration in Jira Server** a couple of days ago.  I hope it helps you too.

Feel free to leave a comment about your experience with the ACP-600 or any other Atlassian certification.

And as a recommendation I do think it works to do what I did and look for the answers yourself in the Atlassian documentation and write them down, as well as doing the labs! But in case you can't find something, here is all the information to the points in the section "**Important concepts to master**" from the Exam Topics.

There is also a Part two of this guide with information about the Jira labs [here](https://mox.sh/sysadmin/acp-600-labs/).

Atlassian links:
* [Atlassian ACP-600 Website Information](https://www.atlassian.com/university/certification/certifications/exam-acp-600)
* [Exam Topics](https://www.atlassian.com/dam/jcr:fe536e84-7bdf-4f4d-8854-4eef93053778/ACP-600%20Exam%20Topics.pdf)

## Important concepts to master

**Exam Topics**: Important concepts to master  
**Issue**: 15.04.2020

## Which configurations are shared VS unique

**Notice:** No matter if we create a project using a template or with shared configuration, we will **NOT** automatically get **`versions`** and **`components`** created in our new project, as well as we will **NOT** share the project description.
{: .notice--danger}

### When new projects are created from template

When we create a project using one of the templates that come with Jira (eg, templates: `Scrum`, `Kanban` and `Basic` in case of the `Software` project type) then **7** schemes will be automatically associated to the new project.  
Some of the schemes are unique to the project and some are shared with other projects:

**Unique schemes:**
* `Issue Type Scheme`
* `Issue Type Screen Scheme`
* `Workflow Scheme`

Unique schemes can be modified and the changes will not affect any other project.

**Shared schemes:**
* `Field Configuration`
* `Notification Scheme`
* `Permission Scheme`
* `Priority Scheme`

Changes to shared schemes will affect other projects.

### When new projects are created with a shared configuration

When we create a project we can check the option: **`Create with shared configuration`**, in other words: copy an existing project.  
**All** of that project's schemes will be shared with the new project, which means that any change to a scheme will affect all the projects using that scheme.

## Who can change a Project Key, and what are the effects on issues in the project

Only **Jira administrators** can change the **`Project Key`**.  
Jira will start a background re-index once the project key is changed. It is limited to the project issues.

### Post-update tasks

**Fix the project entity links:** when Jira is connected to another Atlassian application, entity links would have been automatically created between your Jira projects and the relevant "projects" in other applications, e.g. Confluence spaces. If we change the key of a Jira project, we will need to fix the project entity links.

**Updating Jira Software agile board filters:** if the Jira Software agile boards use the old project key, the board filters may need to be updated to reflect the new project key. Otherwise the board might not display issues from the renamed project.

## Which project details can be added and changed by Project Administrators

As a project admin, we can edit our project's:

* Name
* URL
* Avatar
* Description
* Project lead
* Default assignee
* Sidebar shortcuts

Only **Jira administrators** can edit the **`project key`**, **`project type`**, and **`category`**.

## What are the uses of Project Category in Jira

Project categories are useful in large enterprise instances. The uses are:

* Searching for all the issues in a particular project category using JQL.
* Projects sorted by category.

A Jira project can only belong to one category.

## What features are available for Jira Software vs Jira Core

Jira Software provides the Project types `Software` and `Business`.  
Jira Core provides just the Project type `Business`.

Under those two categories there are available a number of project templates:

* `Software` project type: `Scrum`, `Kanban`, and `Basic`.
* `Business` project type has: `Project management`, `Process management`, and `Task management` templates.

A project template is a set of pre-configurations which will be our starting point when we create a new project.

## What is the importance of the Browse Projects permission

**`Browse Projects`** permission allows users to see projects, view their issues, and search for them in `Issue Navigator`.

It is the most important permission and by default, all logged in users **with** application access have the `Browse Projects` permission.

## How do combinations of permissions work together to enable specific actions

Combinations of permissions are sometimes needed to complete certain actions.

### What permissions are needed to Move Issues

**`Movie Issues`** requires **`Create Issue`** permission in the target's project.

`Move Issues` permission allows to move issues:

* From one project to another.
* From one issue type to another.
* From one workflow to another workflow within the same project.

### What permissions are needed to Edit or Delete All Worklogs

The permissions needed are:

* **`Work On Issues`**
* **`Edit All Worklogs`**
* **`Delete All Worklogs`**

Only relevant if time tracking is enabled.

### What permissions are needed to rank issues in the backlog

The project permissions that are required for **ranking** issues in JIRA Agile Classic Planning Board are:

* **`Schedule Issues`**
* **`Edit Issues`**

The same permissions are also required for **scheduling** issues in JIRA Agile Classic Planning Board.

### What permissions are needed to assign issues to yourself

Two permissions are required to assign issues to ourselfes:

* **`Assign issues`**: permission to assign issues to users. Also allows autocompletion of users in the Assign Issue dropdown.
* **`Assignable user`**: Permission to be assigned issues. This does not include the ability to assign issues.

### What requirements must a user meet in order to use a particular workflow transition

The requirements to use a particular workflow transition are:

* **`Transition Issue`** project permission.
* The user has to meet the workflow **`Conditions`** specified for the transition, if any.

## What is an Issue Collector and what permissions are needed to configure it

**`Issue Collector`** allows us to embed the feedback form into a website.

When the user submits the Jira feedback form, an issue is conveniently created in Jira.  
The permissions required to configure it is, in case of **creating** an `Issue Collector`:

* Jira Administrators Global permission

If we are **editing** an issue collector, we need the following permissions in the project.

* **`Administer projects`**
* **`Create Issues`**
* **`Assignable user`**

## Where are all the places you need to look to fully identify what permissions a particular user has

To identify what permissions a particular user has, we look at:

* **`Application access`**: needed to log in and to use the features of a particular Jira product (Core, Software, Service Desk).
* **`Global permissions`**: system wide permissions that are granted to groups of users.
* **`Permission schemes`**: specify what users can do in a particular project. It is a set of users groups or project roles assignments.
* **`Project roles`**: allow to associate users and/or groups with particular projects.

## What are the use cases where it is best to use Project Roles rather than Groups

* Project roles help to reduce the number of schemes.
* Project administrator can manage membership of the project.
* Changes can be made quicker when membership changes.
* Using project roles reduce Jira administrator's work.

## Which business requirements necessitate the use of separate issue types in a project

Different **`Issue Types`** are needed if:

1. Issues types have different workflows.
2. Issues have different required or hidden fields.
3. Issues need different screens when creating, editing or viewing.
4. And issues need to be reported on separately.

## What is the relationship between screens, screen schemes and issue type screen schemes

A **`screen`** is just a collection of fields.  
Screens are **mapped** to issue **`operations`**: `Create Issue`, `Edit Issue`, and `View Issue`.  
The mapping of `screens` to issue `operations` is called **`screen schemes`** and has to be done by **Jira Administrators**.  
The mapping of `screen schemes` to `issue types` is called **`issue type screen scheme`**.  
And then the `issue type screen scheme` can be applied to one or more projects.

<table>
 <tbody>
  <tr>
        <th colspan="4" style="text-align: center; vertical-align: middle;">Issue Type Screen Scheme</th>
  </tr>

  <tr>
        <th colspan="3" style="text-align: center; vertical-align: middle;">Screen Schemes</th>
        <th colspan="1" style="text-align: center; vertical-align: middle;">Mapped to</th>
  </tr>
  <tr>
        <th>Operation</th>
        <th>Screen</th>
        <th>Screen scheme</th>
        <th style="text-align: center; vertical-align: middle;">Issue Type</th>
  </tr>
  <tr>
        <td>CREATE</td>
        <td>Screen A</td>
        <td>Screen Scheme 1</td>
        <td rowspan="3" style="text-align: center; vertical-align: middle;">Bug<br/>Enhancement</td>
  </tr>
  <tr>
        <td>EDIT</td>
        <td>Screen B</td>
        <td>Screen Scheme 1</td>
  </tr>
  <tr>
        <td>VIEW</td>
        <td>Screen C</td>
        <td>Screen Scheme 1</td>
  </tr>
   <tr>
        <th>Operation</th>
        <th>Screen</th>
        <th>Screen scheme</th>
        <th style="text-align: center; vertical-align: middle;">Issue Type</th>
  </tr>
  <tr>
        <td>CREATE</td>
        <td>Screen D</td>
        <td>Screen Scheme 2</td>
        <td rowspan="3" style="text-align: center; vertical-align: middle;">Task</td>
  </tr>
  <tr>
        <td>EDIT</td>
        <td>Screen D</td>
        <td>Screen Scheme 2</td>
  </tr>
  <tr>
        <td>VIEW</td>
        <td>Screen D</td>
        <td>Screen Scheme 2</td>
  </tr>
 </tbody>
</table>

### How are they related to field configurations

**`Field configurations`** list all the `system fields` and the `custom fields` available in the JIRA instance to be used in `screens`, `schemes`, and `issue schemes`.

A `Field configuration` specifies to JIRA what is the behavior of a `field` when it is applied to an `issue type`. Jira Administrators can change following of a `field`:

* Optional or required
* Hidden or visible
* Description
* Renderer used by the field

Also `Fields` appear in `Transition screens`.

`Transition screens` are used when information is needed as an issue moves through its workflow.

Only **Jira Administrators** can edit `Field configurations` and configure `Transition screens`.
{: .notice--warning}

### How do you determine the maximum number of screens possibly used in a project (for issue operations and workflow transitions)

The maximum number of screens is = ( 3 * number of issue types ) + number of workflow transitions

A `issue type` can have a maximum of **3 screens**, one for each `operation` (create, view and edit).  
A `workflow` in a project can have **1 transition screen** per `transition`.

### Who can make changes to screens, screen schemes and issue type screen schemes

**Info: Project administrators can edit their project’s `screens` as long as they**
* are not shared with any other projects.
* are not a default system screen.
* are not used as a `transition` screen.

**Info: Only Jira Administrators can...**
* create `screens`.
* create/edit `screen schemes`.
* create/edit `issue type screen schemes`.

## What are all the various factors that influence whether a field is visible on a screen

These are the factors that determine if a **`field`** is visible on a **`screen`**:

* The `field` is **not** on the **`screen`**. **Jira Administrators** can remove `fields` from `screens` or the `field` was never added.
* The `field` is **hidden** in the **`field configuration`**.
* The **`field context`** may not be for the right `project` and/or `issue type`.
* The `field` has **no value** yet.
* The `field` has been **hidden by the user** through their `Configure Fields` settings.
* The `user` does not have the right **permission**.

## When is it best to use components, and when is it best to use a custom field instead

**`Components`** allow us to **categorize** `issues` in large groups, for example: `Database`, `Localization`, `Login`, `User Interface`, etc.  
They are optional in a proect.

**`Components`** are used when:

* The `issue` may belong to more than **one** category.
* The `field` value must be managed by the **project administrator**
* The `assignee` must be set based on the selected value. Eg: Tom is `Componente Lead`, we can set then the `Component Lead` as the default asignee.

**`Custom fields`** can be also used to categorize, when:

* The `field` must have a default value.
* The `field` options are shared across projects.

## What are the features, uses, limitations/restrictions of workflow changes, especially the Simplified Workflow

The **`Workflow`** of the project determines which **`statuses`** ara available in that project.

A project uses either a **`Jira Workflow`** or the **`Simplified Workflow`** to control the transitioning of `issues` from one `status` to another.  
All `Jira Software` projects use the `Simplified Workflow` by default.

The `Simplified Workflow` is very basic and it allows `issues` to transition from any status to any other status: **`global transitions`**; either from
inside an issue or by dragging issues on the board.

The `Simplified Workflow` can only be used if a board represents a single project.

### Who can change the Simplified Workflow? What are the limitations on changes to the Simplified Workflow

**Jira Administrators** or **Project Administrators** with **`Extended Project Administration`**.

Users who are **both** **Project Administrators** and **Board Administrators** can change the `Simplified Workflow` via the board configuration.

Once we edit a `Simplified Workflow` adding `statuses` and `transitions` it becomes a `Custom Workflow`.

### What changes can be performed on statuses and transitions

The **`statuses`** can be changed.
The **`transitions`** can also be changed but nothing about `validators` or `conditions` can be modified.

Once we edit a `Simplified Workflow` adding `statuses` and `transitions`, it is no longer a `Simplified Workflow` and becomes a `Custom Workflow`.

A customized workflows is usually a more complex workflow. Often it has a specific sequence of steps rather than allowing every status to transition into every other status (global transitions). The **Jira administrator** may add conditions, user input validation and automated functions to transitions in the workflow.

### What things may need to be updated as a result of workflow changes of any kind

The **`status`** of an `issue` may need to be changed if the original `status`, which the `issue` had, has been removed.

### What workflow changes can be done through the board by Board Administrators

* Add columns to a board and map statuses.
* Switch to the `Simplified Workflow`.

### What types of workflow changes can Project Administrators make if Extended Project Administration is enabled

A Project Administrator needs the `Extended Project Administration` permission in order to edit `workflows` and `screens` in a project.

Project Administrators can edit their project’s workflow if the `workflow`:

* Is not shared with any other projects.
* Is not the Jira default system workflow, which cannot be edited at all.

Project Administrators **cannot** edit the workflow to the same extent as a Jira Administrator. The restrictions are:

* To add a `status`, the status must already exist in the Jira instance i.e. the Project Administrator can't create new statuses or edit existing statuses. However, if the Project Administrator is also a Board Administrator, they can add a new status to the board (which creates a new status in Jira).
* To remove a `status`, the status must not be used by any of the project’s issues.
* The Project Administrator can create, update (name and description) or delete transitions, but they can't select or update a screen used by the transition, or edit or view a transition's properties, conditions, validators or post-functions.

Note: You can end up with a lot of statuses if users can freely add their own statuses to their boards in their projects!
{: .notice--danger}

## How do these settings affect notifications

### AutoWatch setting

**`AutoWatch`** setting controls whether we automatically watch an issue we create or comment on.

### My Changes setting

**`My Changes`** setting tells Jira to email us (or not) when we make a change in an `issue` and we get included in the list of people to be emailed about it.

### Share This Issue

**`Share This Issue`** setting sends e-mails to the people we share with.

### @Mentions

**`@Mentions`** defines whether or not to be notified via e-mail when we are mentioned.

### Filter Subscriptions

**`Filter Subscriptions`** sends us the results of filters on a regular basis.

## What are the configuration options for all the out-of-box reports and gadgets (except Agile reports and gadgets)

### What is their behavior and expected results

There are **three main types of reports**: Agile, Issue analysis, and Forecast & management.

The **`Issue analysis`** and **`Forecast & management`** reports are general reports applicable to all project types for analyzing issues and seeing if your projects are on track.

**`Issue analysis`** reports are:

* **Average Age Report**: Shows the average age of unresolved issues for a project or filter. This helps you see whether your backlog is being kept up to date.
* **Created vs. Resolved Issues Report**: Maps created issues versus resolved issues over a period of time. This can help you understand whether your overall backlog is growing or shrinking.
* **Pie Chart Report**: Shows a pie chart of issues for a project/filter grouped by a specified field. This helps you see the breakdown of a set of issues, at a glance.
* **Recently Created Issues Report**: Shows the number of issues created over a period of time for a project/filter, and how many were resolved. This helps you understand if your team is keeping up with incoming work.
* **Resolution Time Report**: Shows the length of time taken to resolve a set of issues for a project/filter. This helps you identify trends and incidents that you can investigate further.
* **Single Level Group By Report**: Shows issues grouped by a particular field for a filter. This helps you group search results by a field and see the overall status of each group.
* **Time Since Issues Report**: For a date field and project/filter, maps the issues against the date that the field was set. This can help you track how many issues were created, updated, etc, over a period of time.

Configuration Options:
* Project or saved filter
* Period: Hourly, Daily, Weekly,...
* Days previously: 30

**`Forecast & management`** reports are:

* **Time Tracking Report**: Shows the original and current time estimates for issues in the current project. This can help you determine whether work is on track for those issues.
* **User Workload Report**: Shows the time estimates for all unresolved issues assigned to a user across projects. This helps you understand the user's workload better.
* **Version Workload Report**: Shows the time estimates for all unresolved issues assigned to a version, broken down by user and issues. This helps you understand the remaining work for the version.

Configuration Options:
* Version
* Sorting
* Issues
* Subtask inclusion

## How are valid JQL queries written

We need to know what we are searching for:

* **Field name** e.g. `project`
* **Operator** e.g. `equals`, `contains`, etc.
* **Field value** e.g. `Teams in Space`

Query: Return ALL issues in the `Teams in Space` project

Field   Operator  Field Value
project    =    "Teams in Space"

### What operators and arguments are valid for various JQL functions (e.g. membersOf(), StartOfDay(), WAS, CHANGED, etc.)

The list is pretty long so better to check the official docs:

[Advanced Searching Functions](https://confluence.atlassian.com/jirasoftwareserver/advanced-searching-functions-reference-939938746.html)

## Which export options are available from the Issue Navigator

Click **Export** at the top of the page.

Here you see all the different ways you can export your search to reuse in other contexts:

Printable, full content, RSS (Issues), RSS (Comments), CSV (All fields), CSV (Current fields), HTML (All fields), HTML (Current fields), XML, Word, Dashboard Carts

## Bonus points

### Difference between Workflow Conditions and Workflow Validators

**`Conditions`** allow or prevent workflow transitions to be executed. In other words, Transition buttons will not be shown on the `View Issue` page if a condition fails, thus the user will not be able to execute the transition.

Using conditions on workflows are best practices and can be used e.g., as follow:

* Allow only members from a specific group to execute a transition.
* An issue has passed through a required status.
* A field contains a required value.

`Conditions` cannot validate input parameters gathered from the user on the transition's screen. For that we would use `validators`.

**`Validators`** validate the input after the transition button has been clicked, but before the transition has been performed. When they fail, the issue does not go to the destination status of the transition. The transition's post functions are not executed.

Example of validators can be:

* Checking that a field has been updated.
* A date is within a specific range.
