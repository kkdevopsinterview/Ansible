
## 48. How do you deploy the same application across **100 servers using Ansible**?
Deploying the same application across a large number of servers in Ansible is straightforward due to its **parallel execution model** and **inventory-driven targeting**. First, the servers are defined in the inventory file as a group. Then a playbook is written that installs dependencies, copies application files, configures settings, and starts services.

When the playbook runs, Ansible connects to all hosts concurrently using **SSH** and executes tasks using **modules**. Parallel execution is controlled through **forks** to optimize speed and resource usage.

To ensure consistency, **roles and templates** are commonly used so that each server receives identical configurations while still allowing environment-specific variables.

The process typically includes:

1 Define servers in inventory  
2 Create reusable role/playbook  
3 Execute playbook with parallel processing  
4 Validate deployment results  

This ensures scalable, repeatable deployments across large infrastructures.

###  Example
Inventory:

```
[app_servers]
server1
server2
...
server100
```

Playbook:

```yaml
- hosts: app_servers
  roles:
    - deploy_app
```

### Real-World Example
Deploying a web service across hundreds of application nodes behind a load balancer.

### Interview Punch Lines
- “Inventory grouping enables large-scale deployment.”
- “Parallel execution ensures fast rollout.”

---

## 49. What happens if a **task fails** in the middle of a playbook?

When a task fails on a host, Ansible stops executing subsequent tasks for that host but continues execution on other hosts. This prevents cascading failures while allowing unaffected systems to proceed.

However, failure behavior can be customized using **error-handling controls** such as ignoring failures, retry mechanisms, or structured blocks for rescue actions.

Ansible provides flexibility to:

- Stop execution  
- Continue despite errors  
- Trigger recovery logic  

This granular control enables resilient automation workflows.

### Example
```yaml
- block:
    - command: risky_operation
  rescue:
    - debug:
        msg: "Recovery step executed"
```

### Real-World Example
Deployment failure on one server triggers rollback while others continue processing.

### Interview Punch Lines
- “Failures stop execution per host.”
- “Error handling enables recovery.”

---

## 50. How do you perform **rolling updates** using Ansible?

Rolling updates deploy changes gradually across subsets of servers rather than updating all simultaneously. This minimizes service disruption and risk.

Ansible supports rolling updates using **batch execution controls** where only a limited number of hosts are processed at a time. After successful completion, the next batch proceeds.

This strategy ensures system availability and allows quick rollback if issues arise.

### Example
```yaml
- hosts: app_servers
  serial: 10
  roles:
    - deploy_app
```

Updates 10 servers at a time.

### Real-World Example
Updating web servers behind load balancers without affecting user traffic.

### Interview Punch Lines
- “Rolling updates limit deployment batches.”
- “They reduce deployment risk.”

---

## 51. How do you ensure **zero-downtime deployments**?

Zero-downtime deployments focus on maintaining application availability throughout updates. This is achieved by combining **rolling updates** with **load balancer integration**.

Typical process:

1 Remove host from load balancer  
2 Deploy update  
3 Validate health  
4️ Reintroduce host  

Ansible orchestrates this workflow across infrastructure components.

This approach ensures traffic always routed to healthy servers.

### Example Concept
```
tasks:
  - name: Disable node from LB
  - name: Deploy update
  - name: Enable node
```

### Real-World Example
High-traffic platforms updating services without affecting users.

### Interview Punch Lines
- “Zero downtime achieved via orchestration.”
- “Load balancer coordination critical.”

---

## 52. How do you manage **different environments (dev, prod)**?

Environment management is handled through **inventory separation**, **variable scoping**, and **configuration templating**. Different inventories define distinct infrastructure targets, while variable files store environment-specific settings.

This approach allows the same playbook to operate across multiple environments while applying different configurations.

Best practices include:

- Separate inventories  
- Variable hierarchies  
- Role reuse  
- Conditional logic  

This ensures consistency and prevents configuration drift between environments.

### Example
Inventory structure:

```
inventory/
  dev/
  prod/
```

Run:

```bash
ansible-playbook -i inventory/dev site.yml
```

### Real-World Example
Deploying test versions in dev while production uses hardened configs.

### Interview Punch Lines
- “Inventory and variables isolate environments.”
- “One playbook supports multiple deployments.”

---

## 53. Ansible vs **Terraform — when to use which**?

Ansible and Terraform serve different purposes in the infrastructure lifecycle, and understanding when to use each is important.

**Terraform** is an Infrastructure as Code provisioning tool designed to create and manage infrastructure resources such as virtual machines, networks, load balancers, and databases. It works declaratively, defining the desired infrastructure state and reconciling differences through state tracking. Terraform excels at provisioning cloud environments and managing dependencies between resources.

**Ansible**, on the other hand, focuses on configuration management and application deployment. It operates procedurally by executing tasks sequentially and configuring systems after infrastructure exists. It installs packages, deploys applications, modifies configuration files, and manages services.

They are commonly used together in production workflows:

1 Terraform provisions infrastructure  
2 Ansible configures and deploys applications  

Choosing between them depends on task scope:

Provision infrastructure → Terraform  
Configure systems → Ansible  
Full lifecycle → Combine both  

### Example Scenario
Terraform creates virtual machines.  
Ansible installs and configures software on those machines.

### Real-World Example
CI/CD pipeline provisions cloud resources with Terraform and then runs Ansible playbooks to deploy applications.

### Interview Punch Lines
- “Terraform builds infrastructure; Ansible configures it.”
- “They complement each other.”

---

## 54. **Ansible vs Shell Scripting**

Shell scripting and Ansible both automate tasks, but they differ significantly in structure, reliability, and scalability.

Shell scripts execute commands sequentially without awareness of system state. They require manual error handling, lack idempotency, and are harder to maintain at scale.

Ansible provides:

- Structured automation  
- Idempotent modules  
- Inventory management  
- Parallel execution  
- Error handling  

While shell scripts may be useful for quick one-off tasks, Ansible is preferred for enterprise automation and infrastructure management due to maintainability and predictability.

### Example Comparison

Shell:

```
apt install nginx
```

Ansible:

```yaml
apt:
  name: nginx
  state: present
```

### Real-World Example
Large organizations replace shell deployment scripts with Ansible for reliability and auditing.

### Interview Punch Lines
- “Shell executes commands; Ansible manages state.”
- “Ansible scales better.”

---

## 55. Is Ansible **declarative or procedural**?

Ansible is primarily procedural in execution flow but declarative in module behavior, making it a **hybrid model**.

Playbooks define tasks executed step-by-step, which resembles procedural automation. However, modules themselves enforce declarative state management by ensuring systems match desired configurations.

This combination provides flexibility:

- Procedural orchestration  
- Declarative state enforcement  

This hybrid nature allows both sequencing control and configuration consistency.

### Real-World Example
Playbook orchestrates deployment order, while modules ensure package installed only once.

### Interview Punch Lines
- “Ansible is hybrid procedural-declarative.”
- “Flow procedural, modules declarative.”

---

## 56. Can Ansible manage **cloud infrastructure**?

Yes, Ansible can manage cloud infrastructure using provider-specific modules and APIs. It can create instances, networks, storage, and security resources across multiple cloud platforms.

While Terraform specializes in provisioning, Ansible can perform similar tasks and integrate provisioning with configuration workflows in a single tool.

This makes it suitable for:

- Simple provisioning  
- Orchestrated workflows  
- Hybrid automation  

However, Terraform’s state tracking provides stronger lifecycle management for complex infrastructures.

### Example Concept
Playbook provisioning cloud instance using provider modules.

### Real-World Example
Automating virtual machine creation and configuration using one unified playbook.

### Interview Punch Lines
- “Ansible can provision cloud resources.”
- “Terraform better for large infrastructure lifecycle.”

---

## 57. Common **Ansible mistakes in production**

Several mistakes frequently occur in production environments:

- Hardcoding sensitive data instead of using secure storage  
- Using shell commands instead of modules  
- Ignoring idempotency  
- Poor inventory organization  
- Lack of version control  
- Skipping testing and validation  
- Overloading playbooks without modularization  

These mistakes lead to security risks, instability, and maintenance challenges.

Adhering to best practices such as modular roles, encrypted secrets, and structured testing improves reliability.

### Real-World Example
Deployment failure caused by hardcoded credentials leaked into repository.

### Interview Punch Lines
- “Security and structure mistakes common.”
- “Best practices prevent failures.”
