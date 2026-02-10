## 1. What is **Ansible** and how does it work?

**Ansible** is an open-source **automation** and **configuration
management tool** used to provision infrastructure, configure systems,
deploy applications, and orchestrate workflows. It operates using a
simple **push-based model** where a **control node** sends automation
instructions to **managed nodes**.

The automation logic is written in **playbooks** using **YAML syntax**,
which define the desired state of systems. When executed, Ansible
connects to managed nodes, typically over **SSH**, runs **modules**
remotely, and ensures the system matches the defined configuration.

The workflow generally follows this sequence:

1 **Inventory** defines target hosts\
2 **Playbook** describes desired configuration\
3️ **Modules** perform actions\
4️ Results returned to control node

Because it focuses on simplicity and readability, **Ansible** is widely
adopted for **DevOps automation**.

### Example

``` yaml
- hosts: web
  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present
```

### Real-World Example

Using Ansible to configure hundreds of servers after provisioning ---
installing packages, deploying configs, and starting services
automatically.

### Interview Punch Lines

-   "Ansible automates provisioning and configuration."
-   "It uses SSH and YAML playbooks to enforce desired state."

------------------------------------------------------------------------

## 2. How is **Ansible** different from **Chef** and **Puppet**?

**Ansible** differs primarily in **architecture**, **learning curve**,
and **operational complexity**.

Ansible uses an **agentless push model**, meaning no software is
installed on managed nodes --- automation runs directly over **SSH**.
Chef and Puppet typically require **agents installed** on nodes, which
periodically pull configuration from a central server.

Ansible also uses **YAML-based playbooks** that are easy to read and
write. Chef uses **Ruby-based DSL** and Puppet uses **declarative
manifests**, both requiring more specialized learning.

Additionally:

-   Ansible setup is **lightweight**
-   **Faster initial adoption**
-   **Simpler troubleshooting**

Chef/Puppet provide strong scalability and policy-driven enforcement,
but Ansible prioritizes **simplicity** and **rapid automation**.

### Real-World Example

A startup chooses Ansible because onboarding engineers is faster
compared to managing agent-based infrastructure.

### Interview Punch Lines

-   "Ansible is agentless and push-based."
-   "Chef and Puppet rely on agent-driven models."

------------------------------------------------------------------------

## 3. What are **Ansible playbooks**?
## KK FUNDA

**Playbooks** are **YAML files** defining automation workflows. They
describe **tasks** executed on specified hosts to achieve a desired
state.

A playbook contains:

-   Hosts to target
-   Tasks to run
-   Modules to execute
-   Variables and handlers

They provide **repeatable**, **version-controlled automation** and are
the main mechanism for orchestrating system configuration.

Playbooks enable multi-step automation including provisioning,
deployment, and orchestration.

### Example

``` yaml
- hosts: web
  tasks:
    - name: Start service
      service:
        name: nginx
        state: started
```

### Real-World Example

Deploying application stack with multiple sequential steps using a
single playbook.

### Interview Punch Lines

-   "Playbooks define automation workflows."
-   "They orchestrate configuration tasks."

------------------------------------------------------------------------

## 4. What is an **inventory file**?

The **inventory file** defines the list of **managed hosts** and how
they are grouped. It acts as Ansible's source of truth for
infrastructure targets.

Inventories may include:

-   Hostnames/IP addresses\
-   Group definitions\
-   Variables

They can be **static** or **dynamic**.

Grouping enables executing tasks across logical sets of hosts.

### Example

    [web]
    192.168.1.10
    192.168.1.11

### Real-World Example

Grouping application servers separately from database servers.

### Interview Punch Lines

-   "Inventory defines target infrastructure."
-   "It organizes hosts into groups."

------------------------------------------------------------------------

## 5. How does **Ansible communicate** with managed nodes?

Ansible communicates primarily via **SSH** for Linux systems and
**WinRM** for Windows.

The **control node** pushes modules to targets, executes them, and
retrieves results. Communication is secure and does not require
persistent agents.

Authentication methods include:

-   SSH keys
-   Password authentication
-   Kerberos

This design simplifies management.

### Real-World Example

Central automation server managing thousands of hosts remotely.

### Interview Punch Lines

-   "Communication occurs via SSH or WinRM."
-   "No persistent agent required."

------------------------------------------------------------------------

## 6. What is **agentless architecture**?

**Agentless** means managed nodes do not require dedicated software
agents installed. Instead, automation runs directly using existing
communication protocols.

Benefits include:

-   Reduced maintenance
-   Faster setup
-   Lower resource usage
-   Fewer security concerns

This contrasts with agent-based tools needing lifecycle management.

### Real-World Example

Onboarding new servers instantly without installing management software.

### Interview Punch Lines

-   "Agentless reduces operational overhead."
-   "It simplifies infrastructure management."

------------------------------------------------------------------------

## 7. What are **Ansible modules**?

**Modules** are reusable scripts performing specific tasks. Every task
uses a module under the hood.

They abstract system operations and enforce **idempotency**.

Examples include package installs, file operations, service control.

### Example

``` yaml
apt:
  name: nginx
  state: present
```

### Real-World Example

Large automation workflows relying on modules instead of shell scripts.

### Interview Punch Lines

-   "Modules are building blocks of automation."
-   "They ensure reliable execution."

------------------------------------------------------------------------

## 8. What is **YAML** and why used?

**YAML** is a human-readable **data serialization format** used for
defining structured configurations. It relies on indentation rather than
symbols, making playbooks readable.

YAML supports:

-   Lists\
-   Dictionaries\
-   Variables

Its simplicity reduces learning curve and errors.

### Example

``` yaml
name: nginx
state: present
```

### Real-World Example

Engineers reviewing automation easily understand YAML workflows.

### Interview Punch Lines

-   "YAML provides readable automation syntax."
-   "It simplifies configuration authoring."

------------------------------------------------------------------------

## 9. What is an **Ansible role**?

**Roles** organize playbooks into reusable structures with predefined
directory layout.

They contain:

-   Tasks\
-   Variables\
-   Templates\
-   Files

Roles enable modular, scalable automation and reuse across projects.

### Real-World Example

Reusable role installing and configuring web server across environments.

### Interview Punch Lines

-   "Roles modularize automation."
-   "They enable reuse and scalability."

------------------------------------------------------------------------

## 10. What is **idempotency** in Ansible?

**Idempotency** ensures running automation repeatedly produces the same
result without unintended changes.

Modules check system state before acting.

This guarantees:

-   Predictability
-   Safe reruns
-   Stable deployments

### Example

Installing package once despite repeated runs.

### Real-World Example

Continuous compliance automation maintaining configuration consistency.

### Interview Punch Lines

-   "Idempotency ensures consistent outcomes."
-   "Prevents unnecessary changes."
