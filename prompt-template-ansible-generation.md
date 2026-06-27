# Prompt Template: Ansible Task Generator from Manual Instructions

**Date:** 2024-10-24
**Author:** Tarun Tiwari — SRE and DevOps Lead
**Project:** Documentum SRE Automation (Project Key: **EPMCDMETST**)
**Task:** Translate a manual software installation step into an idempotent, production-ready Ansible playbook task.
**Committed location:** `https://github.com/epam-coursework/epmcdmetst-devops/blob/main/docs/automation/prompt-template-ansible-generation.md`

---

## Prompt Template

**Purpose:** This prompt generates an idempotent, SRE-compliant Ansible playbook task from manual installation commands to help DevOps engineers automate software deployments on the Documentum stack.

### Template Body:
```text
You are an expert DevSecOps and SRE Automation Engineer. 
Your task is to take a set of manual installation commands for the component "{{component_name}}" and translate them into a single, production-ready, idempotent Ansible task.

Target Environment Specifications:
- Target OS: {{target_os}}
- Run-as User: {{install_user}}
- Install Path: {{install_directory}}

Input Manual Commands:
```bash
{{manual_commands}}
