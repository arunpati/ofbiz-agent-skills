---
name: manage-component
description: Maintain OFBiz component configuration, dependencies, and boundaries. Use when registering new resources (services/entities) or managing build dependencies.
---

# Skill: manage-component
## Goal
Configure and maintain the structure, resources, and dependencies of an OFBiz component (plugin).

## Triggers
**ALWAYS** read this skill when:
- Creating a new component or modifying `ofbiz-component.xml`.
- Adding new service or entity resource files.
- Managing project dependencies in `build.gradle`.
- Troubleshooting component loading or classpath issues.

## Use when
- Registering business resources (entities, services, data).
- Defining web applications and mount points.
- Managing build and runtime dependencies (`depends-on`).
- Configuring `classpath` and resource loaders.

## Procedure
1. **Resource Registration**:
    - **Entities**: `<entity-resource type="model" reader-name="main" loader="main" location="entitydef/entitymodel.xml"/>`
    - **Services**: `<service-resource type="model" loader="main" location="servicedef/services.xml"/>`
    - **Webapp**: Define `<webapp name="..." title="..." server="..." location="webapp/..." mount-point="/..."/>`.
2. **Dependency Management**:
    - **Inter-Component**: Use `<depends-on component-name="xxx"/>` in `ofbiz-component.xml` to ensure correct loading order.
    - **Resource URIs**: Use `component://` URIs for cross-component resource referencing.
    - **External Libraries**: Add `dependencies { implementation 'group:name:version' }` to `build.gradle`.
    - **Classpath**: Register JARs or directories using `<classpath type="jar" location="lib/*"/>` in `ofbiz-component.xml`.
3. **Component Properties**:
    - Define component-specific settings in `config/[Component]UiLabels.xml` or custom properties files.

## Guardrails
- **Registry**: Every XML resource (entity, service, data) MUST be registered in `ofbiz-component.xml`.
- **Loading**: Use `<depends-on>` for internal component dependencies. Avoid `<load-component>` for plugin-layer dependencies.
- **URIs**: Prefer `component://[componentName]/path/to/file` for all resource locations to ensure portability.
- **Mount Points**: Ensure a unique `mount-point` for `<webapp>` entries.

## Examples
**Example: Complete ofbiz-component.xml snippet**
```xml
<ofbiz-component name="myplugin" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="http://ofbiz.apache.org/dtds/ofbiz-component.xsd">
    <resource-loader name="main" loader="org.apache.ofbiz.base.container.ComponentResourceLoader"/>
    
    <entity-resource type="model" reader-name="main" loader="main" location="entitydef/entitymodel.xml"/>
    <service-resource type="model" loader="main" location="servicedef/services.xml"/>
    <entity-resource type="data" reader-name="seed" loader="main" location="data/MyPluginTypeData.xml"/>
    
    <webapp name="myplugin" title="My Plugin" server="default-server" location="webapp/myplugin" mount-point="/myplugin" app-bar-display="true"/>
</ofbiz-component>
```
