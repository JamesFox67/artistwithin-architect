# System Map

This document describes the structure of the Artist Within AI system and the locations of its major components.

Two workspaces exist in this system:

1. **Architect Workspace** — used by the architect agent (Archy)
2. **Primary System Workspace** — contains the operational system and the operations agent (Ollie)

The architect agent analyzes and designs the system.  
The operations agent performs implementation tasks inside the primary system workspace.

---

# Architect Workspace

Location:

/home/jamesfox/.openclaw/workspace-architect

Purpose:

This workspace belongs to **Archy**, the system architect agent.  
It contains Archy’s identity, rules, and memory.

Archy reads system structure from this document and analyzes the **Primary System Workspace** when designing architecture or issuing task specifications.

Archy generally **does not modify system files directly**.

---

# Primary System Workspace

Location:

/home/jamesfox/.openclaw/workspace

This is the **main operational workspace** where the Artist Within automation system lives.

It contains:

- the CRM application
- operational scripts
- documentation
- the operations agent workspace

Key files:

- AGENTS.md — agent governance rules
- SOUL.md — agent personality and thinking style
- USER.md — human context
- MEMORY.md — curated long-term memory
- memory/ — daily session logs
- TOOLS.md — operational notes and environment details

The operations agent (Ollie) primarily works inside this workspace.

---

# Application Project

Directory:

artistwithin_crm/

This directory contains the **core CRM application** used to manage leads and marketing operations.

Architecture:

- Django web application
- PostgreSQL database
- Redis
- Celery worker
- Nginx reverse proxy

Purpose:

Manage contacts, leads, marketing data, and operational workflows.

---

# Data Operations

artistwithin-crm-db-backups/

Stores database backups for the CRM system.

scripts/

Contains operational scripts used for tasks such as:

- data ingestion
- scraping
- automation utilities

---

# Documentation

docs/

Contains design notes and operational documentation including:

- scraping architecture
- repository references
- OpenClaw usage notes

---

# Agent Roles

## Archy — System Architect

Responsibilities:

- analyze the system structure
- interpret human requests
- convert ideas into task specifications
- define guardrails for implementation
- protect long-term system architecture

Archy focuses on **design and coordination**, not direct implementation.

---

## Ollie — Operations Agent

Responsibilities:

- implement tasks defined by Archy
- operate within the primary system workspace
- modify code, scripts, and configuration as instructed
- perform operational workflows

Ollie executes tasks but does not define architecture.
