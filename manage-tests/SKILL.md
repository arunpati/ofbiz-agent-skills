---
name: manage-tests
description:
- Defining automated test suites in `testdef/`.
- Implementing JUnit or Groovy tests.
- Running tests via `./gradlew test` or `./gradlew "ofbiz --test"`.
---

# Skill: manage-tests
## Goal
Define and maintain automated test suites within the OFBiz framework to ensure code quality and regression safety.

## Triggers
**ALWAYS** read this skill when:
- Creating or modifying XML files in `testdef/` directory (e.g., `tests.xml`).
- Writing new integration tests for services or entities.

## Use when
- Adding specialized test cases for new business logic.
- Debugging failing automation suites.
- Verifying data integrity via entity tests.

## Procedure
1. **Identify Test Type**:
    - `entity-test`: For verifying entity operations and data model constraints.
    - `service-test`: For testing service execution and result maps.
    - `junit-test-suite`: For running complex Java-based JUnit tests.
2. **Define Test Suite**:
    - Use `<test-suite>` as the root element.
3. **Registration**: Ensure the test suite is registered in `ofbiz-component.xml`:
   ```xml
   <test-suite loader="main" location="testdef/MyTestSuite.xml"/>
   ```
4. **Data Isolation**:
    - Use `<entity-xml>` in test defs to load specific test data.
    - Prefer using `Seed` or `Demo` data foundations.
5. **Assertions**: Use standard JUnit assertions (`assertEquals`, `assertNotNull`) or `ServiceUtil.isSuccess(result)`.
6. **Execution**:
    - Run tests using Gradle: `./gradlew "ofbiz --test component=[component-name] --test suitename=tests"`.

## Guardrails
- **Isolation**: Ensure tests do not depend on external data states. Use seed or demo data provided within the component.
- **Naming**: Test case names should be descriptive of the scenario (e.g., `testCreateExample`).
- **Paths**: Use `component://` paths for all resource locations.

## Examples
**Example: Simple XML Service Test**
```xml
<test-suite xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <service-test service-name="createExample"/>
    <entity-test entity-name="Example"/>
</test-suite>
```
