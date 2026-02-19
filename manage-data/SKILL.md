---
name: manage-data
description: |
  Create and manage OFBiz data files (Entity Engine XML). Use when
  - Adding Seed/Demo data to the system.
  - Fixing foreign key violations in data files.
  - Configuring entity data resources in `ofbiz-component.xml`.
---

# Skill: manage-data (v1.0)

## Goal
Create and manage OFBiz data files (Entity Engine XML) to populate the database with correct and valid seed, demo, or configuration data.

## Triggers
**ALWAYS** read this skill when:
- Creating or updating XML files intended for data loading.
- Troubleshooting data-related startup or component errors.

## Use when
- **Seed Data**: Mandatory data required for the application to function (e.g., system settings).
- **Type/Metadata Data**: Common enums, status items, and type definitions.
- **Demo/Test Data**: Optional data for development and testing environments.
- **Job Scheduling**: Background tasks using `JobSandbox` and `RecurrenceInfo`.

## Procedure
1. **Header**: ALWAYS include the correct XSI schema for validation:
   ```xml
   <entity-engine-xml xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:noNamespaceSchemaLocation="http://ofbiz.apache.org/dtds/entity-engine-xml.xsd">
   ```
2. **Entries**: Define records using the entity name as the tag. Group elements by entity for readability.
   ```xml
   <Product productId="PROD_001" productTypeId="FINISHED_GOOD" internalName="Sample Product"/>
   ```
3. **Registration**: Register the data file in `ofbiz-component.xml`:
   ```xml
   <entity-resource type="data" reader-name="seed" loader="main" location="data/MyData.xml"/>
   ```
    - `reader-name`: Use `seed` for mandatory data, `seed-initial` for setup, or `demo` for sample data.

## Guardrails
- **Ordering**: Define parent entities BEFORE child entities to avoid foreign key errors during load.
- **Dates**: Use ISO-8601 format: `YYYY-MM-DD HH:MM:SS.SSS` (e.g., `2023-01-01 00:00:00.000`).
- **CDATA**: Wrap text containing XML special characters (e.g., descriptions with HTML) in `<![CDATA[ ... ]]>`.
- **Primary Keys**: Ensure PKs are unique. For Seed/Ext data, use descriptive string IDs if possible.

## Examples
**Example: Service Job Scheduling**
```xml
<RecurrenceInfo recurrenceInfoId="9000" startDateTime="2023-01-01 00:00:00.000" recurrenceInfoTypeId="DAILY"/>
<JobSandbox jobId="9000" jobName="Custom Daily Task" runtimeDataId="" serviceName="myCustomService"
    poolId="pool" runTime="2023-01-01 00:00:00.000" recurrenceInfoId="9000" statusId="SERVICE_PENDING"/>
```
