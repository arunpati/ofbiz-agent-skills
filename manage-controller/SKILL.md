---
name: manage-controller
description: Manage OFBiz Web Controller definitions. Use when defining requests, redirects, and view mappings.
---

# Skill: manage-controller
## Goal
Define and maintain request mappings and view definitions within the OFBiz `controller.xml`.

## Triggers
**ALWAYS** read this skill when:
- Modifying `WEB-INF/controller.xml`.
- Registering a web application in `ofbiz-component.xml`.
- Adding new UI pages (views) or form submission targets (requests).

## Use when
- Designing URL structures for the web application.
- Mapping requests to Java/Groovy events or OFBiz services.
- Configuring security (HTTPS, Auth) for specific URLs.

## Procedure
1. **Security & Authentication**:
    - Set `uri` to a unique identifier.
    - Set `https="true"` for sensitive data.
    - Set `auth="true"` to require user login.
2. **Register**: Ensure the webapp is registered in `ofbiz-component.xml`:
   ```xml
   <webapp name="myApp" title="My App" server="default-server" location="webapp/myApp" mount-point="/myapp"/>
   ```
3. **Request Mapping**:
    - Define `<event type="..." path="..." invoke="..."/>` for processing logic.
    - Common event types: `java`, `service`, `groovy`.
4. **Response Handling**:
    - Use `<response name="success" type="view" value="..."/>` for standard page rendering.
    - Use `<response name="success" type="request" value="..."/>` for request chaining (redirects).
    - Use `<response name="error" type="view" value="..."/>` for returning to the input page on failure.
5. **View Definitions**:
    - Map the `value` in the response to a `<view-map>`.
    - Specify `type="screen"` for Screen Widget views.
    - Set `page="..."` to the component location of the screen (e.g., `component://example/widget/example/ExampleScreens.xml#EditExample`).

## Guardrails
- **Naming**: Request URIs should be camelCase (e.g., `updateExample`).
- **Chaining**: Avoid deep request chains; prefer direct redirects (`type="request-redirect"`).
- **Redundancy**: Check for global-requests and global-responses in standard OFBiz controllers to avoid duplication.

## Examples
**Example: Standard Request with Service Event and View Mapping**
```xml
<request-map uri="updateExample">
    <security https="true" auth="true"/>
    <event type="service" invoke="updateExample"/>
    <response name="success" type="view" value="EditExample"/>
    <response name="error" type="view" value="EditExample"/>
</request-map>

<view-map name="EditExample" type="screen" page="component://example/widget/example/ExampleScreens.xml#EditExample"/>
```
