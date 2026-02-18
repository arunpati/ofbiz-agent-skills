# OFBiz Agent Skills Plugin

## Overall Goal
The `ofbiz-skills` plugin is a repository of standardized knowledge, best practices, and implementation patterns for Apache OFBiz. Its primary purpose is to empower AI agents (like Antigravity) to perform complex OFBiz development tasks—such as creating entities, defining services, building screens, and managing business logic—with high accuracy and adherence to framework-specific guardrails.

By providing these "skills" in a structured format, we ensure that agents follow the correct design patterns (e.g., favoring View Entities over manual iteration, using Worker classes correctly, adhering to security standards) without manual intervention for every step.

## How to Use with an Agent
AI agents can internalize these skills by reading the `SKILL.md` files located in each sub-component directory. 

### Instructions for the Agent
To utilize these skills, provide the following instruction in the agent's prompt or context:
> "Whenever performing tasks related to OFBiz components (e.g., Screens, Entities, Services, Data), first read the relevant skill definitions located in `plugins/ofbiz-skills/[skill-directory]/SKILL.md`. Adhere to the triggers, procedures, and guardrails defined therein."

### Integration Patterns
- **Triggered Reading**: Instruct the agent to "trigger" a skill read when it detects a specific file type extension (e.g., `.rest.xml` triggers `manage-api-integration`).
- **Contextual Loading**: At the start of a feature development task, the agent should list the `ofbiz-skills` directory and select relevant knowledge bases to load.

## Directory Structure
These skills should be placed within the `plugins` directory of your OFBiz framework:
`/path/to/ofbiz/plugins/ofbiz-skills/`

Each directory contains:
- `SKILL.md`: The core knowledge file containing the goal, procedures, and guardrails for that specific skill.

## Available Skills
For a detailed list of all skills with short descriptions, see [SKILLS_SUMMARY.md](file:///Users/arun/personal/arun/ofbiz_dev/ofbiz-framework/plugins/ofbiz-skills/SKILLS_SUMMARY.md).

The toolkit currently covers:
- **Core Abstractions**: `manage-entities`, `manage-services`, `manage-data`.
- **UI & Interaction**: `manage-screens`, `manage-forms`, `manage-menus`, `manage-templates`, `manage-ajax`.
- **Logic & Flows**: `manage-groovy`, `manage-java`, `manage-java-patterns`, `manage-eca`, `manage-service-groups`.
- **Integrations**: `manage-api-integration`, `manage-email-services`.
- **Advanced Management**: `manage-security-advanced`, `manage-localization-advanced`, `manage-webapps`, `manage-cache-and-performance`.
- **Strategies**: `manage-strategies` (Xml vs Java/Groovy, Dos and Donts).

## Deployment and Updates
This plugin is linked to the GitHub repository: `https://github.com/arunpati/ofbiz-agent-skills.git`. To update the skills available to your agent, pull the latest changes from the repository.
