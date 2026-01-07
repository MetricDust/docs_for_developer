# Rules for frontend developers which should be followed Mandatorily

## PR and Task Rules

This document explains **how developers and reviewers should work using Azure Boards and Pull Requests**. The goal is clarity, consistency, and zero confusion.

---

## Sprint Board Structure

### Board Columns

The sprint board contains the following columns:

* **(No Heading Column)** – Used for the **Story / Main Task**
* **To Do** – Tasks ready to be picked up (State: New)
* **In Progress** – Tasks actively being worked on
* **Blocked** – Tasks that cannot continue due to issues or dependencies
* **Done** – Completed tasks
* **Revision** – Used when a PR is rejected and changes are required

### Story and Tasks

* A **Story** is the main requirement or feature
* Each story is **divided into multiple tasks**
* Developers work on **tasks**, not directly on stories
* Ideally:
  * Story and tasks belong to the **same repository**
  * Story and tasks use the **same branch**

---

## For Developers

### Before Starting Any Work

1. **Check Task Assignment**
   * Verify the task is **assigned to you**
   * Do not start work on unassigned tasks
2. **Clear Any Doubts Early**
   * If requirements are unclear:
     * Ask questions immediately
     * Get clarity before writing any code
3. **Check the Sprint Board**
   * Review all columns (To Do, In Progress, Blocked, Done)
4. **Understand the Story First**
   * Open the story (main task)
   * Read and understand the requirement clearly
   * Do not start development without clarity
5. **Repository and Branch Linking (Mandatory)**
   * Verify the story is linked to the correct repository and branch
   * If not linked:
     * Create a branch immediately
     * Link the story to that branch
   * Link all related tasks to the same branch

---

### Working on Tasks

1. **Initial State**
   * Tasks will be available in the **To Do** column
   * Task state should be **New**
2. **Start Work**
   * Move the task from **To Do → In Progress**
3. **Blocked Tasks**
   * If work cannot continue:
     * Move the task to **Blocked**
     * Add a comment explaining the reason
4. **Complete Task**
   * After finishing development and committing changes:
     * Move the task to **Done**

---

### Completing the Story

1. **Verify All Tasks**
   * Ensure all tasks under the story are in **Done** state
2. **Manual Version Upgrade (Mandatory)**
   * Once tasks are completed and before resolving the story:
     * Manually upgrade the required version (Major / Minor / Patch)
     * Ensure version bump follows team/versioning rules
3. **Resolve the Story**
   * Change the story state to **Resolved** using the state dropdown

---

### Pull Request Rules

1. **Create PR**
   * PR must be created only after the story is in **Resolved** state
2. **PR Comment Requirement (Mandatory)**
   * In the PR comments, explicitly mention:
     * The **version upgrade performed**
     * Example: `Version bumped from v1.2.3 → v1.2.4`
3. **Reviewers (Mandatory)**
   * Add a minimum of **2 reviewers**
4. **Approval and Merge**
   * Ensure reviewers have:
     * Added comments
     * Approved the PR
   * Merge only to **main / master** branch
5. **Post Merge**
   * Run the pipeline after merge

---

### If PR Is Rejected

1. **Board Updates**
   * Move the task to **Revision** column
   * Change the story state to **Active**
2. **Fix and Resubmit**
   * Address all review comments
   * Push changes to the **same branch**
   * Create a **new PR**

---

## For Reviewers

### Before Reviewing the PR

1. **Check Tasks**
   * Confirm all tasks under the story are completed
2. **Check Story State**
   * Story must be in **Resolved** state
   * If not resolved, do not proceed with approval

---

### Reviewing the PR

1. **Version Verification (Mandatory)**
   * Check that the **version has been manually upgraded**
   * Verify the version change mentioned in PR comments
   * If version is missing or incorrect:
     * Reject the PR
     * Ask for proper version update
2. **Code Review**
   * Review logic, structure, and correctness
   * Ensure implementation matches the story requirements
3. **Approve or Reject**
   * If code is correct:
     * Approve the PR
     * Add comments
   * If issues exist:
     * Reject the PR
     * Add clear and actionable comments

---

### If PR Is Rejected

1. **Board Actions**
   * Move the task to **Revision** column or ask developer to move the task to **Revision** column
   * Change the story state to **Active** or ask developer to change the story state to **Active**
2. **Feedback Quality**
   * Comments should be specific and easy to act on

---

## Final Notes

* Board status must always reflect reality
* No resolved story without completed tasks
* No merge without approval

Follow the process to keep delivery clean and predictable.

---

## Front end Coding practices

-   Code must be **logs free** before being deployed or committed.
-   Code must not have **unwanted commented function or variables**. But can have comments for documentation purpose.
-   Function must follow a particular flow in **hierarchical manner**. Like first function which lodes when page is loaded must come first and others must be one below other in the hierarchy.
-   Names of variable and function must be self explanatory and with proper formate either in **camelCase** or **snake_case** and it must be followed through the code.
-   Variable must be assigned with **proper type** of it like string, boolean, number etc.
-   Proper **comments** must be used for functions parameters and return type when ever needed or the name for the **params can be self explanatory**.
-   Every **if** condition must be followed with **else**. when there is nothing in else block it can be have just comments
-   **deprecated syntax** must be avoided and if any must be changed to proper syntax.
-   Class and styles must be maintained as mush as in generic way and those can be kept in **styles.scss** file.
-   Try to make the page **stateless** as musch as possible using queryparams or path params. **preferred query params**
-   Where ever the id or any other params is required is taken form the **query params it must be single variable** and must be using same variable when needed.
-   Use of observables can be reduces and **signals** can be used as alternative.
-   Local storage must be **secured or encrypted** and must clear them on logout or closed.
-   **git ignore** must be containing unwanted files which is not required in the cloud like node_modules, logs, vscode settings, etc.
-   Services must be maintained for api calls and generic functions. generic function can be added in `common.service.ts` file and respective services file can be made for other use cases.
-   Its best to maintain **2 levels of parent child hierarchy in components**, at max 3 levels not more. And if its 3 levels the api call or control of data must be always maintained in the parent or the last child not in the middle.

## The pattern of the component ts file must be followed as bellow.

```typescript
import { Component } from "@angular/core";
import { FormBuilder, FormGroup, Validators } from "@angular/forms";
// other imports will be here

@Component({
    selector: "app-sample",
    templateUrl: "./sample.component.html",
    styleUrls: ["./sample.component.css"],
})
export class SampleComponent {
    // all the injectable will be at first
    _service = inject(SampleService);

    // sample variables will be second
    sample_variables: type;

    // constructor will be third
    constructor(private fb: FormBuilder) {}

    // ngOnInit must be forth
    ngOnInit() {
        // respective code

        // function must be called in hierarchical manner
        this.function_1();
        this.function_2();
    }

    // onChanges function must be fifth
    onChanges() {
        // respective code
    }

    // reset function must be sixth if its required
    reset() {
        // respective code
    }

    function_1() {
        // respective code
    }

    function_2() {
        // respective code
        this.function_3();
    }

    function_3() {
        // respective code
    }

    // submit function must be the last one before generic function
    submit() {}

    // last must be the generic function with comment as bellow
    // generic function
    function_4() {
        return something;
    }
}
```

This pattern will help in code review and debugging.

### Command to change CRLF to LF in bash scripts

```bash
sed -i 's/\r//' deploy_to_beta.sh
sed -i 's/\r//' deploy_to_prod.sh
sed -i 's/\r//' deploy.sh
```
