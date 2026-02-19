---
name: manage-groovy
description: |
  Write Groovy logic for OFBiz services and scripts. Use when
  - Writing logic for OFBiz services using Groovy.
  - Creating UI/API data preparation scripts.
  - Implementing modern entity logic using Groovy DSL.
---

# Skill: manage-groovy (v1.0)

## Goal
Implement business logic in Groovy using modern OFBiz DSL patterns.

## Triggers
**ALWAYS** read this skill when:
- Creating or modifying `.groovy` files.
- Implementing logic for services with `engine="groovy"`.

## Use when
- Writing core business logic.
- Complex data transformations or filtering.
- Data preparation for screen/form context.

## Procedure
1. **Service vs. Script**:
    - **Service**: Methods like `def myService() { ... }` that return a Map.
    - **Script**: Flat logic for UI data preparation (returns context variables).
2. **Entity DSL**: Use the modern OFBiz DSL for readability:
   ```groovy
   List products = from("Product").where("statusId", "PROD_ACTIVE").orderBy("productName").queryList()
   GenericValue product = from("Product").where("productId", productId).queryOne()
   ```
3. **Service Call**:
   ```groovy
   Map result = runService("updateProduct", [productId: productId, internalName: "New Name"])
   ```
4. **Data Manipulation**:
    - Leverage Groovy's power: `list.collect { it.name }`, `map.subMap(['key1', 'key2'])`.
5. **Context Access**:
    - Services access `parameters`. Scripts access global bindings (e.g., `context`).
6. **Success/Error**:
    - Use `return success([key: val])` or `return error("Message")`.

## Guardrails
- **Naming**: Use camelCase for methods and variables.
- **Security**: Always use `auth="true"` in the service definition. Groovy scripts inherit the security context of the dispatcher.
- **Typing**: Prefer `def` for local dynamics, but use explicit types (e.g., `GenericValue`, `Map`) for public API/Service interfaces.
- **Performance**: Avoid `findList` if a simple `queryOne` or `queryList` with strict `where` clauses is possible.

## Examples
**Example: Groovy Service**
```groovy
import org.apache.ofbiz.entity.GenericValue

def updateInternalName() {
    String productId = parameters.productId
    GenericValue product = from("Product").where("productId", productId).queryOne()
    if (product) {
        product.internalName = parameters.internalName
        product.store()
        return success("Product name updated")
    }
    return error("Product not found")
}
```
