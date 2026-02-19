---
name: manage-services
description: Define and implement OFBiz services. Use when creating new services in services.xml, modifying attributes, or debugging dispatcher.
---

# Skill: manage-services
## Goal
Define and implement business logic as reusable, transactional OFBiz services.

## Triggers
**ALWAYS** read this skill when:
- Defining or modifying `servicedef/services.xml`.
- Registering services in `ofbiz-component.xml`.
- Implementing service logic in Java, Groovy, or MiniLang.
- Troubleshooting service execution/validation.

## Use when
- Exposing business logic to the UI or API.
- Orchestrating complex data workflows.
- Handling transactional operations.

## Procedure
1. **Engine Selection**:
    - `entity-auto`: Use for standard CRUD (Create, Update, Delete). Highly efficient.
    - `groovy`: Use for logic requiring entity queries, map manipulation, or UI preparation.
    - `java`: Use for core performance-critical logic or complex integrations.
2. **Service Definition**:
    - **In/Out Parameters**: Use `<attribute name="..." mode="IN|OUT|INOUT" type="..." optional="true|false"/>`.
    - **Auth**: Set `auth="true"` for protected services.
    - **Export**: Set `export="true"` to expose the service to external callers.
    - **Action**: Set `action="GET|POST|PUT|DELETE"` to automatically export the service as a REST API endpoint.
    - **Transactions**: Default is `use-transaction="true"`. Use `require-new-transaction="true"` for independent operations.
3. **Register**: Ensure the `services.xml` is registered in `ofbiz-component.xml`:
   ```xml
   <service-resource type="model" loader="main" location="servicedef/services.xml"/>
   ```
4. **Execution Logic**:
    - **Request Processing**: Access parameters via `context` (Map).
    - **Results**: Return results via `ServiceUtil.returnSuccess()` or `returnError()`.
4. **Service Overrides**:
    - Use `<implements service="..." />` to inherit attributes from an existing service.

## Guardrails
- **Naming**: Use `verbNoun` (e.g., `updateExample`).
- **Persistence**: Do not handle database transactions manually in code; let the service engine manage them.
- **Security**: ALWAYS implement permission checks. Use `<permission-service>` in `services.xml` or `security.hasEntityPermission()` in implementation.
- **Contract**: Services MUST return success/error maps via `ServiceUtil` (Java) or DSL shorthands (Groovy).
- **Null Safety**: Always check for mandatory attributes in implemention if not strictly enforced by XML.
- **Error Messages**: Use properties from `uiLabelMap` for user-facing error messages.

## Examples
**Example: REST Export in Service Definition**
```xml
<service name="findProductById" engine="java" auth="true" export="true" action="GET"
         location="org.apache.ofbiz.product.product.ProductServices" invoke="findProductById">
    <description>Automatically exported as a REST API</description>
    <attribute name="productId" mode="IN" type="String" optional="false"/>
</service>
```

**Example: CRUD with entity-auto**
```xml
<service name="createExample" engine="entity-auto" invoke="create" default-entity-name="Example" auth="true">
    <auto-attributes mode="IN" include="nonpk" optional="true"/>
    <auto-attributes mode="OUT" include="pk" optional="false"/>
</service>
```

**Example: Service implementation in Groovy**
```groovy
def updateExample() {
    Map result = ServiceUtil.returnSuccess()
    GenericValue example = from("Example").where("exampleId", context.exampleId).queryOne()
    if (example) {
        example.setNonPKFields(context)
        example.store()
    }
    return result
}
```
