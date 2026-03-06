---
name: manage-cache-and-performance
description: Manage OFBiz caching strategies using UtilCache and Entity Engine caching to optimize application performance.
---

# Manage Cache and Performance Skill

This skill provides patterns for implementing and configuring caching in OFBiz to improve performance and reduce database load.

## Caching with `UtilCache`

`UtilCache` is the primary caching utility in OFBiz. It supports LRU (Least Recently Used) eviction, expiration based on time, and soft references.

### Pattern: Programmatic Caching
```java
// Get or create a cache instance
UtilCache<String, GenericValue> myCache = UtilCache.getOrCreateUtilCache("my.custom.cache", 1000, 1000, 3600000, false);

// Putting a value in cache
myCache.put("myKey", myValue);

// Retrieving from cache
GenericValue cachedValue = myCache.get("myKey");

// Removing from cache
myCache.remove("myKey");
```

## Cache Configuration (`cache.properties`)

Caches are configured in `framework/base/config/cache.properties`. Use this file to define limits and expiration.

### Configuration Pattern
```properties
# Set max entries (0 for unlimited)
my.custom.cache.maxSize=5000

# Set expire time in milliseconds (e.g., 1 hour = 3600000)
my.custom.cache.expireTime=3600000

# Use soft references (allows GC to reclaim memory if needed)
my.custom.cache.useSoftReference=true
```

## Entity Engine Caching

OFBiz automatically caches entity lookups if configured.

### Declarative Caching
In `entitymodel.xml`:
```xml
<entity entity-name="MyEntity" package-name="..." use-cache="true">
    ...
</entity>
```

### Programmatic Caching (Delegator)
Even if `use-cache="false"` at the entity level, you can force it in code:
```java
// queryOne with cache
GenericValue val = EntityQuery.use(delegator).from("Product").where("productId", "10000").cache(true).queryOne();

// queryList with cache
List<GenericValue> list = EntityQuery.use(delegator).from("ProductCategory").cache(true).queryList();
```

## Service Results Caching

Services can cache their results automatically based on input parameters.

### Pattern: Service Caching
In `services.xml`:
```xml
<service name="calculateComplexPrice" engine="java" ... use-cache="true">
    <description>Caches results based on input parameters</description>
    ...
</service>
```

## Performance Monitoring & Best Practices

1. **Monitor via WebTools**: Use the "Cache Maintenance" page in WebTools to check hit/miss ratios.
2. **Cache Selective Entities**: Only cache entities that are frequently read but rarely updated.
3. **Avoid Over-Caching**: Excessive caching can lead to `OutOfMemoryError`. Use `useSoftReference=true` for large caches.
4. **Manual Clearing**: Use `UtilCache.clearCache(name)` when data updates occur outside normal OFBiz entity flows (e.g., direct SQL).
5. **DDS (Distributed Data Services)**: For multi-node setups, ensure cache clearing is synchronized (usually via RMI or JMS in OFBiz).
