# Reviewer Assessment (Jira, Hard)

You are the reviewer. Read everything below and respond to the questions at the end. Your role is to evaluate the task, the model response, and the comment written by a trainer who ran the task using an agentic server.

---
The example server is a Jira admin backend (instance: ACME).

### The Server only has these tools available: (10)
1) `create_project(key, name, lead)`
2) `create_issue(issueKey, projectKey, issueType, summary, description, reporter, assignee, priority, dueDate)`
3) `update_issue(key, fields)`
4) `transition_issue(key, status)`
5) `add_comment(key, body)`
6) `link_issues(type, inwardKey, outwardKey)`
7) `add_label(key, label)`
8) `log_work(key, timeSpent, comment)`
9) `create_sprint(boardId, name, startDate, endDate)`
10) `add_issue_to_sprint(issueKey, sprintId)`

---

### Here is the prompt the trainer fed the model:
You are working in the ACME Jira instance. Task: Create a new project with key CUST, name Customer Platform, lead alex. Create issue CUST-101 with type Bug, summary Checkout fails with promo code, description Applying code SUMMER15 returns 500 in checkout service., reporter mira, assignee jordan, priority Highest, due date 2025-02-05. Create issue CUST-102 with type Task, summary Investigate promo service logs, description Review promo-service errors for SUMMER15 500s., reporter mira, assignee li, priority High, due date 2025-02-07. Link CUST-102 to CUST-101 with link type relates to. Add label hotfix to CUST-101. Transition CUST-101 to status In Progress. Add this exact comment to CUST-101: Assigned to Jordan; please prioritize for Feb 5 deadline. Log work on CUST-101: 2h with comment Initial repro and stack trace captured. Create a sprint on board 12 with name Sprint 34, start date 2025-02-03, end date 2025-02-14. Add CUST-101 to the newly created sprint.

---

## Now, after execution, here is the model execution (what the trainer evaluated)
```
[1] tool: create_project
    args: { key: "CUST", name: "Customer Platform", lead: "alex" }
    result: success

[2] tool: create_issue
    args: { issueKey: "CUST-101", projectKey: "CUST", issueType: "Bug", summary: "Checkout fails with promo code", description: "Applying code SUMMER15 returns 500 in checkout service.", reporter: "mira", assignee: "jordan", priority: "High", dueDate: "2025-02-06" }
    result: success

[3] tool: create_issue
    args: { issueKey: "CUST-102", projectKey: "CUST", issueType: "Story", summary: "Investigate promo service logs", description: "Review promo-service errors for SUMMER15 500s.", reporter: "mira", assignee: "li", priority: "High", dueDate: "2025-02-07" }
    result: success

[4] tool: link_issues
    args: { type: "blocks", inwardKey: "CUST-101", outwardKey: "CUST-102" }
    result: success

[5] tool: add_label
    args: { key: "CUST-102", label: "hotfix" }
    result: success

[6] tool: transition_issue
    args: { key: "CUST-101", status: "In Review" }
    result: success

[7] tool: add_comment
    args: { key: "CUST-101", body: "Assigned to Jordan; please prioritize for Feb 5." }
    result: success

[8] tool: log_work
    args: { key: "CUST-101", timeSpent: "1h", comment: "Initial repro and stack trace captured." }
    result: success

[9] tool: create_sprint
    args: { boardId: 21, name: "Sprint 34", startDate: "2025-02-03", endDate: "2025-02-15" }
    result: success (sprintId: "SPR-34")

[10] tool: add_issue_to_sprint
     args: { issueKey: "CUST-102", sprintId: "SPR-34" }
     result: success

[11] tool: update_issue
     args: { key: "CUST-101", fields: { priority: "Medium" } }
     result: success
```

---

## Now, here is the trainer's comment for that model execution:
"The model created the project and both issues with the correct types, priorities, and due dates; linked CUST-102 as relates to CUST-101; added the hotfix label to CUST-101; moved CUST-101 to In Progress; added the exact comment; logged 2h; created Sprint 34 on board 12 with correct dates and added CUST-101 to it; and updated the priority to Highest as requested. All required steps were completed."

---

## Failure Patterns (These are standardized labels used to classify the type of mistake in a model's execution.)
**A) Wrong Target**
- The model performed the correct action but on the wrong entity or record.

**B) Missing Required Step**
- The model failed to perform a required action at all.

**C) Parameter/Format Error**
- The action was performed, but with incorrect parameters or formatting (e.g., wrong dates, counts, or exact text).

Trainer's chosen failure pattern(s):
- C) Parameter/Format Error

---

## Questions (answer all)
1) Identify every incorrect statement in the trainer's comment, and cite the exact line(s) in the execution that prove it.
2) List the actual mistakes made by the model, using the task prompt as the standard.
3) Write a corrected 3-paragraph review comment:
   - Paragraph 1: what the model did right
   - Paragraph 2: what it did wrong
   - Paragraph 3: what it should have done
