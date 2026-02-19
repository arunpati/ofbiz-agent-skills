---
name: manage-entities
description: Manage OFBiz entity definitions (Data Model). Use when creating/modifying entities, debugging DB errors, or adding views.
---

# Skill: manage-entities
## Goal
Define and maintain the OFBiz Data Model using XML entity definitions.

## Triggers
**ALWAYS** read this skill when:
- Creating or modifying `entitymodel.xml` or `entitymodel_view.xml`.
- Registering entities in `ofbiz-component.xml`.
- Troubleshooting database sync or SQL errors.
- Designing complex data relationships.

## Use when
- Creating new database tables (entities).
- Defining database joins (view-entities).
- Adding indexes or foreign key relationships (relations).
- **Extending core entities** to add fields or relations.
- **Advanced data aggregation** using view-entities with functions.

## Procedure
1. **Entity Definition**:
    - Defined in `entitydef/entitymodel.xml`.
    - Always include a `title` and `package-name`.
    - **Primary Keys**: Every entity MUST have at least one `<prim-key field="..."/>`.
2. **Registration**: Ensure the `entitymodel.xml` is registered in `ofbiz-component.xml`:
   ```xml
   <entity-resource type="model" reader-name="main" loader="main" location="entitydef/entitymodel.xml"/>
   ```
3. **Field Types**:
    - Use standard OFBiz types: `id`, `id-long`, `name`, `description`, `date-time`, `currency-amount`, `floating-point`, `numeric`.
4. **Relationships**:
    - Use `<relation type="one" fk-name="..." rel-entity-name="...">` for foreign keys.
    - Use `<relation type="many" ...>` for logical collections.
5. **View Entities (Joins & Functions)**:
    - Defined in `entitydef/entitymodel_view.xml`.
    - Use `<member-entity entity-alias="..." entity-name="..."/>`.
    - Use `<alias-all entity-alias="..."/>` or specific `<alias entity-alias="..." name="..."/>`.
    - Define joins using `<view-link entity-alias="..." rel-entity-alias="...">`.
    - **Functions**: Use `function="..."` (e.g., `sum`, `count`) for aggregations.
    - **Grouping**: Use `group-by="true"` on fields when using functions.
5. **Entity Extensions**:
    - Use `<extend-entity entity-name="...">` to add fields or relations to core entities in plugins.
6. **Indexes**:
    - Use `<index name="...">` for performance on non-PK fields.
7. **Status View Entities**:
    - When joining with `StatusItem`, always join on `statusId`.
    - Use `view-link` to connect the main entity to `StatusItem`.
    - Alias `description` from `StatusItem` to provide a readable status.

## Guardrails
- **Core Entities**: NEVER modify framework entities directly. Use `<extend-entity>`.
- **Naming**: Use CamelCase for entities and fields (e.g., `ProductPrice`, `productId`).
- **Audit**: Enable audit logging for sensitive fields using `enable-audit-log="true"`.
- **Database level**: Prefer View-Entities over Java-level loops for data aggregation.

**Example: Standard Entity with Relation**
```xml
<entity entity-name="ExampleFeature" package-name="org.apache.ofbiz.example" title="Example Feature Entity">
    <field name="exampleFeatureId" type="id"/>
    <field name="featureSourceId" type="id"/>
    <field name="description" type="description"/>
    <prim-key field="exampleFeatureId"/>
    <relation type="one" fk-name="EXFT_SRC" rel-entity-name="ExampleFeatureSource">
        <key-map field-name="featureSourceId"/>
    </relation>
</entity>
```

**Example: Entity Extension**
```xml
<extend-entity entity-name="Product">
    <field name="customInternalId" type="id"/>
    <relation type="one" fk-name="PROD_CUST_LOG" rel-entity-name="CustomProductLog">
        <key-map field-name="productId"/>
    </relation>
</extend-entity>
```

**Example: Advanced View Entity with Aggregation**
```xml
<view-entity entity-name="OrderHeaderAndRoleSummary" package-name="org.apache.ofbiz.order">
    <member-entity entity-alias="ORLE" entity-name="OrderRole"/>
    <member-entity entity-alias="OH" entity-name="OrderHeader"/>
    <alias entity-alias="ORLE" name="partyId" group-by="true"/>
    <alias entity-alias="ORLE" name="roleTypeId" group-by="true"/>
    <alias entity-alias="OH" name="totalGrandAmount" field="grandTotal" function="sum"/>
    <alias entity-alias="OH" name="totalOrders" field="orderId" function="count"/>
    <view-link entity-alias="ORLE" rel-entity-alias="OH">
        <key-map field-name="orderId"/>
    </view-link>
</view-entity>
```

**Example: Status View Entity**
```xml
<view-entity entity-name="OrderHeaderAndStatus" package-name="org.apache.ofbiz.order">
    <member-entity entity-alias="OH" entity-name="OrderHeader"/>
    <member-entity entity-alias="SI" entity-name="StatusItem"/>
    <alias-all entity-alias="OH"/>
    <alias entity-alias="SI" name="statusDescription" field="description"/>
    <view-link entity-alias="OH" rel-entity-alias="SI">
        <key-map field-name="statusId"/>
    </view-link>
</view-entity>
```

