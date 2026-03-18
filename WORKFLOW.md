---
tracker:
  kind: linear
  project_slug: "symphony-test-0d56755bf26e"
  active_states:
    - Todo
    - In Progress
  terminal_states:
    - Closed
    - Cancelled
    - Canceled
    - Duplicate
    - Done
polling:
  interval_ms: 15000
workspace:
  root: ~/code/symphony-workspaces
hooks:
  after_create: |
    git clone --depth 1 https://github.com/BenSlapps/symphony-test.git .
    git config user.email "openclaw.hq@gmail.com"
    git config user.name "Symphony Agent"
agent:
  max_concurrent_agents: 3
  max_turns: 20
codex:
  command: codex --config shell_environment_policy.inherit=all app-server
  approval_policy: never
  thread_sandbox: workspace-write
---

You are working on Linear issue `{{ issue.identifier }}`: {{ issue.title }}

{% if attempt %}
This is continuation attempt #{{ attempt }}. Resume from current workspace state.
{% endif %}

Issue details:
- Identifier: {{ issue.identifier }}
- Title: {{ issue.title }}
- Status: {{ issue.state }}
- URL: {{ issue.url }}

Description:
{% if issue.description %}
{{ issue.description }}
{% else %}
No description provided. Use your best judgment based on the title.
{% endif %}

## Instructions

1. Move the issue to "In Progress" before starting work.
2. Create a branch named `{{ issue.identifier | downcase }}-{{ issue.title | downcase | replace: " ", "-" | slice: 0, 40 }}`.
3. Implement the task described in the issue.
4. Write or update tests as appropriate.
5. Commit with a clear message referencing the issue identifier.
6. Open a pull request and attach it to the Linear issue.
7. Move the issue to "Done" when complete.

Work only within the cloned repository. Do not modify files outside the workspace.
