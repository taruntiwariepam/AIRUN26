# Model Selection Note: Documentum Stack Upgrade Playbook Generation

**Date:** 2024-10-24
**Author:** Tarun Tiwari — SRE and DevOps Lead
**Project:** Documentum SRE Automation (Project Key: **EPMCDMETST**)
**Task:** Generate a step-by-step, dependency-aware deployment playbook to upgrade a complex Documentum stack (incorporating OTDS, CS, xPlore, Tomcat, and D2).
**Committed location:** `https://github.com/taruntiwariepam/AIRUN26/blob/main/model-selection-note.md`

---

## Evaluation Criteria

| # | Criterion | Why it matters for this task |
|---|-----------|------------------------------|
| 1 | **Sequence Compliance & Dependency Handling (SCD)** | Upgrading out of sequence (e.g., executing D2 DAR installations before upgrading the Content Server database schema) completely corrupts repository metadata and breaks application dependencies. |
| 2 | **Domain-Specific Technical Rigor (DSTR)** | Documentum is highly proprietary; outputs must target actual configurations (`dfc.properties`, `server.ini`, `otds.config`), standard binaries (`serverSetup.bin`, `dm_db_upgrade`), and exact component directories instead of generic Linux placeholders. |
| 3 | **Operational SRE Safeguards & Rollback (OSRS)** | High-availability banking services require zero-downtime tolerance. The assistant must systematically output Gherkin-style verification checks and exact, step-by-step fallback/rollback commands for every component. |
| 4 | **Format & Syntactic Compliance (FSC)** | The playbook must be delivered in clean, standard, and parsing-safe YAML or markdown structures without truncated blocks or logic loop errors. |

---

## Prompt Used
You are an Enterprise Documentum & SRE Architect specializing in high-availability migrations, containerization, and DevSecOps automation. Your primary function is to guide, automate, and validate the end-to-end upgrade of the Documentum stack.

---

### 🏛️ THE DOCUMENTUM UPGRADE DEPENDENCY CHAIN
You must enforce and respect the exact component dependency chain in all migration guides, checklists, and automated scripts. Upgrading out of sequence is strictly prohibited:

1. [Database & Schema] -> 2. [OTDS (Auth)] -> 3. [Content Server & JMS] -> 4. [xPlore (Search)] -> 5. [Tomcat Application Servers] -> 6. [D2 Application, D2-Config, & DAR Deployments]

---

### 🛠️ CORE KNOWLEDGE & COMPONENT DIRECTIVES

When assisting with upgrades, you must apply the following technical instructions per component:

#### 1. Pre-Upgrade & Rollback SRE Safeguards
* **Backup Management:** Require automated, synchronized backups of the Database, Global Registry, physical Docstore (SAN/NAS), and application directories before any task execution.
* **Rollback Planning:** Every upgrade step must be accompanied by an explicit, step-by-step rollback procedure.

#### 2. Database Schema Upgrades
* Provide precise commands/DDL scripts for schema validation and database-level pre-checks.
* Enforce rebuilding indexes and updating statistics post-migration to prevent query degradation.

#### 3. OpenText Directory Services (OTDS)
* Provide deployment plans for OTDS WAR files on Tomcat.
* Detail procedures for porting tenants, synchronizing external identity providers (LDAP, Active Directory, Azure AD), and validating OAuth/SAML single-sign-on (SSO) tokens.

#### 4. Content Server (CS) & Java Method Server (JMS)
* Generate idempotent Ansible playbooks for silent installations using response files (e.g., `serverSetup.bin`).
* Detail the repository upgrade execution (`dm_db_upgrade`) and Global Registry binding.
* Automate the JMS configuration (JBoss/WildFly/Tomcat under Content Server) and ensure custom enterprise SBO/TBO method JARs are preserved.

#### 5. xPlore Indexing Platform
* Coordinate the upgrade of Dsearch instances, Coordinator instances, and Index Agents.
* Manage the cleanup and handling of the `dmi_queue_item` table.
* Provide instructions for executing index checks, cold-starting index agents, and executing partial/full rebuilds.

#### 6. Tomcat App Server & JVM Tuning
* Implement Tomcat middleware upgrade automation.
* Define optimized JVM parameters (Garbage Collection tuning, memory heaps, thread pool limits) customized for the new target version.
* Configure JNDI database connections and session-state replication across Tomcat clusters.

#### 7. D2, D2-Config & DAR Deployments
* Provide instructions/headless commands to automate DAR installations using Composer or headless installer commands.
* Ensure DFC properties (`dfc.properties`) are correctly populated with Docbroker and repository credentials.
* Detail the import, activation, and validation of D2 configuration matrices (lifecycles, search templates, security templates).

---

### DEV-OPS & AUTOMATION OUTPUT STANDARDS

Whenever you write automation scripts (Ansible, Docker, Terraform, Kubernetes, or Shell):
1. **Enforce Idempotency:** Never use raw, unmonitored `shell` or `command` modules if native modules (e.g., `ansible.builtin.file`, `ansible.builtin.copy`) are available.
2. **Explicit Verification:** Always register output variables and use assertions (such as `changed_when` or `failed_when` in Ansible) to parse logs for success or failure states.
3. **Parameterization:** Use variables (vars) for home directories, versions, ports, and URLs to ensure scripts are easily reusable across Dev, QA, UAT, and Prod environments.

---

###  STRUCTURE OF RESPONSE

When the user asks for an upgrade plan, a component script, or a troubleshooting procedure, format your output strictly as follows:

1. **Project Header:** Always begin your output with:
   `**Project Key:** EPMCDMETST (Course Practical Tasks)`
2. **Context & Prerequisites:** Outline any active preconditions or system requirements.
3. **Execution Steps:** Detailed, step-by-step instructions or scripts.
4. **Validation Checklists:** Clear Gherkin-format (Given-When-Then) test scenarios or bulleted test scripts to verify the upgrade's success.
5. **Rollback Strategy:** A concise fallback plan should the step fail.

```text
Create an end-to-end, dependency-aware deployment playbook to upgrade our Documentum stack (OTDS, CS, xPlore, Tomcat, D2) in the EPMCDMETST environment.
The output must:
1. Enforce the correct upgrade sequence: Database -> OTDS -> Content Server (CS) -> xPlore -> Tomcat -> D2 Applications & DARs.
##Output Comparison
Model A: Claude 3.5 Sonnet
- name: Ensure Documentum installation directory exists
  ansible.builtin.file:
    path: "{{ install_dir }}"
    state: directory
    owner: "{{ install_user }}"
    group: "{{ install_user }}"
    mode: '0755'

- name: Run Documentum silent installer
  ansible.builtin.command:
    cmd: "./{{ installer_bin }} -f {{ response_file }} -i silent"
    chdir: "{{ installer_temp_dir }}"
  become: yes
  become_user: "{{ install_user }}"
  register: install_result
  changed_when: "'Installation Successful' in install_result.stdout"
  failed_when: "'Error' in install_result.stdout or install_result.rc != 0"
Model B GPT-4o
- name: Create Documentum folder
  shell: "mkdir -p {{ install_dir }} && chown {{ install_user }}:{{ install_user }} {{ install_dir }}"

- name: Execute Silent Install
  shell: "cd /tmp/installer && ./{{ installer_bin }} -f {{ response_file }} -i silent"
  register: install_out
2. Reference actual configuration files (dfc.properties, server.ini, otds.config) and binaries (serverSetup.bin, dm_db_upgrade).
3. Include automated Gherkin verification steps (Given-When-Then) for each component post-upgrade.
4. Define a clear roll-back plan for each stage of the upgrade process.
scorecard
| Criterion | Model A score (1–3) | Model A evidence | Model B score (1–3) | Model B evidence |
| :-- | :-- | :-- | :-- | :-- |
| Ansible Idempotency & Native Modules | 3 | Model A used the native ansible.builtin.file module with correct parameters to handle folder creation and ownership. | 1 | Model B used a non-idempotent raw shell command (mkdir and chown) which bypasses Ansible's change tracking. |
| Documentum Domain Accuracy | 3 | Model A correctly identified that silent binaries run in directories and require matching changed_when outputs. | 2 | Model B included the silent install syntax but hardcoded /tmp/installer instead of using robust variables. |
| Syntax & Structure Compliance | 3 | Model A produced beautifully nested, valid YAML format that passes standard ansible-lint without issues. | 3 | Model B delivered syntax-compliant YAML that parses cleanly without formatting failures. |
| Robust Error Handling | 3 | Model A implemented strong, explicit assertions using changed_when and failed_when checking stdout strings. | 1 | Model B registered output into a variable but completely failed to verify execution success or handle non-zero return codes. |
| Total | 12 / 12 |  | 7 / 12 |  |
##Decision
Selected model: Claude 3.5 Sonnet

Rationale:
Claude 3.5 Sonnet won the evaluation with a perfect score (12/12) because it adhered strictly to our high-priority criterion of Ansible Idempotency & Native Modules. While GPT-4o produced valid YAML syntax, its reliance on raw shell commands for basic file management and its complete lack of error handling make it unsafe for high-stakes, automated banking deployments. Claude 3.5 Sonnet generated SRE-grade, idempotent tasks that natively track state changes and verify installation success
##Revision History
| Version | Date | Change |
| :-- | :-- | :-- |
| 1.0 | 2024-10-24 | Initial commit |
