## 11. What are **Ansible modules** and why are they important?

**Ansible modules** are reusable, standalone units of code that perform
specific automation tasks on managed nodes. They are the core building
blocks of **Ansible playbooks**. Every task in a playbook ultimately
invokes a module --- whether it's installing packages, managing files,
starting services, or interacting with cloud APIs.

Modules are executed on remote hosts through **SSH (or APIs)**, perform
the requested operation, and return structured **JSON output**
describing success, failure, and changes made. This abstraction allows
engineers to automate complex operations without writing low-level
scripts.

Modules are important because they:

-   Provide **standardized automation functionality**
-   Reduce need for **custom scripting**
-   Ensure **idempotency and predictability**
-   Improve **readability and maintainability**
-   Support **cross-platform automation**

Without modules, Ansible would essentially be executing raw shell
commands, losing reliability and structure.

###  Example

``` yaml
- name: Install nginx
  hosts: web
  tasks:
    - name: Install package
      apt:
        name: nginx
        state: present
```

Uses **apt module**.

### Real-World Example

Automating provisioning of hundreds of servers where modules handle
package installs, configuration, and service startup consistently.

### Interview Punch Lines

-   "Modules are the functional units of Ansible automation."
-   "They provide reliable and reusable Demonstrations of tasks."

------------------------------------------------------------------------

##  12. How do modules differ from **shell** or **command modules**?

Standard Ansible modules perform structured operations with built-in
logic, validation, and **idempotency**. They understand the target
system and ensure desired state without unnecessary changes.

**Shell and command modules**, however, simply execute arbitrary
commands on remote systems. They lack awareness of system state and do
not provide idempotency or structured behavior.

Key differences:

-   Modules are **state-driven**
-   Shell/command are **execution-driven**
-   Modules are **safer and predictable**
-   Shell/command are **flexible but risky**

Best practice is always to use purpose-built modules when available and
fallback to shell/command only when necessary.

### Example

Preferred:

``` yaml
apt:
  name: nginx
  state: present
```

Less ideal:

``` yaml
shell: apt install nginx
```

### Real-World Example

Using module ensures package installed only once, while shell may
reinstall repeatedly.

### Interview Punch Lines

-   "Modules manage state; shell executes commands."
-   "Modules provide safety and idempotency."

------------------------------------------------------------------------

## 13. Difference between **command** and **shell module**?

Both execute commands on remote hosts, but they differ in how commands
are interpreted.

The **command module** runs commands directly without invoking a shell.
This makes it safer and avoids shell interpretation issues.

The **shell module** runs commands through a shell interpreter, allowing
use of shell features like:

-   Pipes
-   Redirection
-   Variables
-   Wildcards

Because shell interprets input, it introduces security and
predictability risks.

### Example

Command:

``` yaml
- command: ls /tmp
```

Shell:

``` yaml
- shell: cat file.txt | grep error
```

### Real-World Example

Using command for simple execution, shell for complex scripting logic.

### Interview Punch Lines

-   "command executes directly."
-   "shell enables shell features but adds risk."

------------------------------------------------------------------------

## 14. When should you avoid using **shell module**?

Avoid shell module when an equivalent **Ansible module** exists, because
shell bypasses idempotency and introduces potential security
vulnerabilities like **command injection**.

Shell usage should be minimized when:

-   Managing packages
-   Editing files
-   Managing services
-   Configuring users

It should be reserved for edge cases where no module supports required
functionality.

### Real-World Example

Installing packages via shell causes repeated execution instead of state
verification.

### Interview Punch Lines

-   "Use shell only as last resort."
-   "Prefer built-in modules for safety."

------------------------------------------------------------------------

## 15. Commonly used **built-in modules**

Ansible provides hundreds of **built-in modules**. Some widely used ones
include:

-   Package management modules
-   File management modules
-   Service management modules
-   User/group modules
-   Copy/template modules

These cover most infrastructure automation needs.

### Example

``` yaml
apt:
copy:
file:
service:
user:
```

### Real-World Example

Typical server provisioning uses combination of package install, config
deployment, and service management modules.

### Interview Punch Lines

-   "Built-in modules cover most automation tasks."
-   "They eliminate need for custom scripts."

------------------------------------------------------------------------

## 16. How does Ansible ensure **idempotency** using modules?

**Idempotency** means running the same playbook multiple times produces
the same result without unintended changes.

Modules check current state before applying changes. If system already
matches desired state, module reports no change.

This behavior ensures:

-   Predictable automation
-   Safe re-runs
-   Reduced downtime

Shell commands lack this feature.

### Example

``` yaml
apt:
  name: nginx
  state: present
```

Installs only if absent.

### Real-World Example

Playbooks run nightly ensuring servers remain compliant.

### Interview Punch Lines

-   "Modules enforce desired state."
-   "They prevent unnecessary changes."

------------------------------------------------------------------------

## 17. What is **copy module**?

The **copy module** transfers files from control node to managed hosts.
It ensures destination file matches source content.

Used for:

-   Config deployment
-   Script distribution
-   Static files

Supports permissions and ownership control.

### Example

``` yaml
- copy:
    src: config.conf
    dest: /etc/app/config.conf
```

### Real-World Example

Deploying configuration files across cluster nodes.

### Interview Punch Lines

-   "Transfers files to hosts."
-   "Ensures consistent configuration distribution."

------------------------------------------------------------------------

## 18. How does **template module** work?

**Template module** deploys files using dynamic variables processed
through templating engine.

It enables generating customized configurations per host.

Useful for environment-specific configs.

### Example

``` yaml
- template:
    src: config.j2
    dest: /etc/app/config.conf
```

### Real-World Example

Generating server configs with unique IP or hostname values.

### Interview Punch Lines

-   "Template creates dynamic configs."
-   "Supports environment customization."

------------------------------------------------------------------------

## 19. What is **file module**?

**File module** manages filesystem objects and their properties.

It can:

-   Create/delete files
-   Manage permissions
-   Create directories
-   Set ownership

Provides structured filesystem management.

### Example

``` yaml
- file:
    path: /data/logs
    state: directory
```

### Real-World Example

Preparing directory structure before deployments.

### Interview Punch Lines

-   "Manages filesystem state."
-   "Controls permissions and structure."

------------------------------------------------------------------------

## 20. What is **service module**?

**Service module** controls system services.

It can:

-   Start
-   Stop
-   Restart
-   Enable at boot

Works across different init systems.

### Example

``` yaml
- service:
    name: nginx
    state: started
    enabled: true
```

### Real-World Example

Ensuring web services running after deployment.

### Interview Punch Lines

-   "Manages service lifecycle."
-   "Ensures availability."
