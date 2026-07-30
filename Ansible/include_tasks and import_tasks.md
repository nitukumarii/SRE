# Difference Between `include_tasks` and `import_tasks` in Ansible

Both `include_tasks` and `import_tasks` are used to split large playbooks into smaller, reusable task files. The key difference is **when Ansible loads those tasks**.

| Feature | `include_tasks` | `import_tasks` |
|----------|-----------------|----------------|
| Loading Type | Dynamic | Static |
| Evaluation | Runtime | Parse Time (before execution) |
| Conditional Support | Applied at runtime | Applied when playbook is parsed |
| Can Use Variables in File Name | Yes | No (must be known at parse time) |
| Best For | Dynamic execution | Static and predictable execution |

---

# 1. `import_tasks` (Static Import)

`import_tasks` loads the task file **before the playbook starts executing**. During the parsing phase, Ansible treats the imported tasks as if they were written directly inside the playbook.

### Example

**main.yml**

```yaml
---
- hosts: all

  tasks:

    - import_tasks: install_nginx.yml
```

**install_nginx.yml**

```yaml
- name: Install Nginx
  yum:
    name: nginx
    state: present

- name: Start Nginx
  service:
    name: nginx
    state: started
```

### Characteristics

- Loaded before execution.
- Good for fixed workflows.
- Better performance because tasks are parsed only once.
- File name must be known before execution.

---

# 2. `include_tasks` (Dynamic Include)

`include_tasks` loads the task file **during playbook execution (runtime)**.

It allows decisions to be made based on variables, conditions, or facts collected during execution.

### Example

```yaml
---
- hosts: all

  tasks:

    - include_tasks: "{{ ansible_os_family }}.yml"
```

If the host is RedHat:

```
RedHat.yml
```

If the host is Debian:

```
Debian.yml
```

This is possible because the operating system is detected at runtime.

---

# Example Using `when`

```yaml
tasks:

- include_tasks: install_nginx.yml
  when: ansible_os_family == "RedHat"
```

Here, the task file is included only if the condition is true during execution.

---

# Production Example

Suppose your company supports both:

- Ubuntu Servers
- RHEL Servers

Directory:

```
tasks/

Ubuntu.yml

RedHat.yml
```

Main playbook:

```yaml
tasks:

- include_tasks: "{{ ansible_os_family }}.yml"
```

When the playbook runs:

- Ubuntu servers execute `Ubuntu.yml`
- RHEL servers execute `RedHat.yml`

This is a common production pattern because one playbook can support multiple operating systems.

---

# When to Use `import_tasks`

Use `import_tasks` when:

- Task file is always required.
- Workflow is fixed.
- No runtime decision is needed.
- Better readability and slightly faster execution.

Example:

```
Always install Java
Always configure Nginx
Always deploy application
```

---

# When to Use `include_tasks`

Use `include_tasks` when:

- Tasks depend on variables.
- Tasks depend on operating system.
- Tasks depend on environment (Dev/QA/Prod).
- Dynamic decision-making is required.

Example:

```
Deploy different configuration for Dev and Production.
```

---

# Interview Answer (Senior Level)

> **`import_tasks` is a static import mechanism. Ansible loads the task file during the playbook parsing phase, before execution begins. It is suitable when the workflow is fixed and every task file must always be executed.**
>
> **`include_tasks` is a dynamic include mechanism. The task file is loaded during runtime, allowing decisions based on variables, host facts, or conditions. This makes it ideal for environments where different hosts require different task files, such as supporting multiple operating systems or environments.**
>
> **In production, I typically use `import_tasks` for common tasks that every server must execute and `include_tasks` for OS-specific or environment-specific tasks where runtime flexibility is needed.**

---

# Quick Revision

| Use Case | Recommended |
|-----------|-------------|
| Fixed workflow | `import_tasks` |
| Runtime decision | `include_tasks` |
| OS-specific tasks | `include_tasks` |
| Environment-specific tasks | `include_tasks` |
| Always execute same tasks | `import_tasks` |
| Variable-based file names | `include_tasks` |

---

# One-Line Interview Answer

> **`import_tasks` loads task files at parse time (static), while `include_tasks` loads them at runtime (dynamic). I use `import_tasks` for fixed workflows and `include_tasks` when task execution depends on variables, conditions, or host facts.**
