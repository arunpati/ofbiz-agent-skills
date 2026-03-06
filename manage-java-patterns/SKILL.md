---
name: manage-java-patterns
description: Follow OFBiz Java coding patterns. Use when creating Helper or Worker classes for business logic and data retrieval.
---

# Skill: manage-java-patterns
## Goal
Implement business logic and data retrieval using consistent OFBiz Java patterns, distinguishing between Helper and Worker roles.

## Triggers
**ALWAYS** read this skill when:
- Creating a new Java class in an OFBiz component.
- Refactoring service logic into reusable classes.
- Implementing complex data retrieval from entities.

## Use when
- Creating `*ReadHelper` classes for entity data encapsulation.
- Creating `*Worker` classes for static utility logic (e.g., calculations, validations).
- Handling `GenericValue` and `Delegator` in Java code.

## Procedure
1. **Helper Pattern (`ReadHelper`)**:
    - **Purpose**: Object-oriented wrapper around a `GenericValue` or a set of related entities.
    - **Structure**: Constructor takes a `GenericValue` (e.g., `OrderHeader`) or a `Delegator`.
    - **Usage**: Encapsulates common queries related to that object (e.g., `getShippingAddress()`, `getTotalAmount()`).
    - **Example**: `OrderReadHelper`, `ShoppingCartHelper`.
2. **Worker Pattern (`Worker`)**:
    - **Purpose**: Static utility classes for generic business logic or UI-related tools.
    - **Structure**: Private constructor (non-instantiable). All methods are `public static`.
    - **Usage**: Common calculations, formatting, or checks (e.g., `ProductWorker.isPhysical()`).
    - **Example**: `ProductWorker`, `ContactMechWorker`, `EntityUtil`.
3. **Data Retrieval Strategy**:
    - Use `EntityQuery` for fluent API queries (including primary key lookups).
    - Handle `GenericEntityException` with `Debug.logError`.

## Guardrails
- **Naming**: Ensure suffixes `Helper` or `Worker` are used appropriately.
- **Statelessness**: Worker classes MUST be stateless.
- **Nesting**: Avoid deep nesting of helper methods; favor readability and performance.

## Examples
**Example: ReadHelper Pattern**
```java
public class MyOrderHelper {
    private GenericValue orderHeader;
    public MyOrderHelper(GenericValue orderHeader) { this.orderHeader = orderHeader; }

    public List<GenericValue> getItems() throws GenericEntityException {
        return orderHeader.getRelated("OrderItem", null, null, false);
    }
}
```

**Example: Worker Pattern**
```java
public class MyProductWorker {
    private MyProductWorker() {} // Private constructor

    public static boolean isDigital(GenericValue product) {
        return "DIGITAL_GOOD".equals(product.getString("productTypeId"));
    }
}
```
