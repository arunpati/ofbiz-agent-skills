# OFBiz Agent Skills Plugin

## Overall Goal
The `ofbiz-skills` plugin is a repository of standardized knowledge, best practices, and implementation patterns for Apache OFBiz. Its primary purpose is to empower AI agents (like Antigravity) to perform complex OFBiz development tasks—such as creating entities, defining services, building screens, and managing business logic—with high accuracy and adherence to framework-specific guardrails.

By providing these "skills" in a structured format, we ensure that agents follow the correct design patterns (e.g., favoring View Entities over manual iteration, using Worker classes correctly, adhering to security standards) without manual intervention for every step.

## Agent Setup & Activation

For an AI agent (like Antigravity) to effectively use these skills, they need to be accessible in specific locations.

### 1. Framework Location (Plugin)
Keep the source of these skills in your OFBiz plugins directory for version control:
`~/ofbiz-framework/plugins/ofbiz-skills/`

### 2. Native Agent Directory (Activation)
For the agent to recognize these as **Core Skills** (allowing tools like `view_skill` to work), they must be placed in the agent's native directory:
`~/.gemini/antigravity/skills/`

### 🛠️ Recommended Setup: Symlinking
To maintain the skills in your GitHub-tracked plugin while enabling them for the agent, use symbolic links:

```bash
# Example: Activate the manage-quartz-jobs skill
ln -s ~/ofbiz-framework/plugins/ofbiz-skills/manage-quartz-jobs ~/.gemini/antigravity/skills/manage-quartz-jobs
```

## How to Use with an Agent
Once activated or shared, AI agents can internalize these skills by reading the `SKILL.md` files.

### Instructions for the Agent
If not natively activated, provide the following instruction:
> "Whenever performing tasks related to OFBiz components, first read the relevant skill definitions located in `plugins/ofbiz-skills/[skill-directory]/SKILL.md`."

### Integration Patterns
- **Triggered Reading**: Instruct the agent to "trigger" a skill read when it detects a specific file type extension (e.g., `.rest.xml` triggers `manage-api-integration`).
- **Contextual Loading**: At the start of a task, the agent should list the `ofbiz-skills` directory and load relevant knowledge.

## Directory Structure
These skills should be placed within the `plugins` directory of your OFBiz framework:
`/path/to/ofbiz/plugins/ofbiz-skills/`

Each directory contains:
- `SKILL.md`: The core knowledge file containing the goal, procedures, and guardrails for that specific skill.

## Available Skills
For a detailed list of all skills with short descriptions, see [SKILLS_SUMMARY.md](~/ofbiz-skills/SKILLS_SUMMARY.md).

The toolkit currently covers:
- **Core Abstractions**: `manage-entities`, `manage-services`, `manage-data`.
- **UI & Interaction**: `manage-screens`, `manage-forms`, `manage-menus`, `manage-templates`, `manage-ajax`.
- **Logic & Flows**: `manage-groovy`, `manage-java`, `manage-java-patterns`, `manage-eca`, `manage-service-groups`.
- **Integrations**: `manage-api-integration`, `manage-email-services`.
- **Advanced Management**: `manage-security-advanced`, `manage-localization-advanced`, `manage-webapps`, `manage-cache-and-performance`.
- **Strategies**: `manage-strategies` (Xml vs Java/Groovy, Dos and Donts).

## Deployment and Updates
This plugin is linked to the GitHub repository: `https://github.com/arunpati/ofbiz-agent-skills.git`. To update the skills available to your agent, pull the latest changes from the repository.
