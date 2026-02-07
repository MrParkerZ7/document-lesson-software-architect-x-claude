# Caching

> **Navigation**: [Back to Lesson Overview](../README.md) | [Previous: CAP Theorem & Consistency](../04-cap-theorem-consistency/README.md) | [Next: Storage Solutions](../06-storage-solutions/README.md)

---

## Caching Strategies

```
+-------------------------------------------------------------+
|                 Caching Patterns                             |
|                                                             |
|  Cache-Aside (Lazy Loading):                               |
|  +------+   1. Check cache                                 |
|  |Client|---------------------->+-------+                   |
|  |      |<----------------------| Cache |                   |
|  |      |   2. Cache miss       +-------+                   |
|  |      |---------------------->+-------+                   |
|  |      |   3. Get from DB      |  DB   |                   |
|  |      |<----------------------+-------+                   |
|  |      |   4. Update cache                                |
|  |      |---------------------->+-------+                   |
|  +------+                       | Cache |                   |
|                                 +-------+                   |
|                                                             |
|  Write-Through:                                            |
|  - Write to cache and DB synchronously                     |
|  - Ensures cache consistency                               |
|  - Higher write latency                                    |
|                                                             |
|  Write-Behind (Write-Back):                                |
|  - Write to cache, async write to DB                       |
|  - Lower write latency                                     |
|  - Risk of data loss                                       |
+-------------------------------------------------------------+
```

## Cache Invalidation

| Strategy | Description | Use Case |
|----------|-------------|----------|
| **TTL** | Expire after time | Session data |
| **Event-based** | Invalidate on data change | Real-time updates |
| **Version-based** | Include version in key | Configuration |
| **LRU** | Remove least recently used | Memory management |

## Redis Caching Example

```python
import redis
import json
from functools import wraps

redis_client = redis.Redis(host='localhost', port=6379, db=0)

def cache(ttl_seconds=300):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            # Generate cache key
            key = f"{func.__name__}:{args}:{kwargs}"

            # Check cache
            cached = redis_client.get(key)
            if cached:
                return json.loads(cached)

            # Execute function
            result = func(*args, **kwargs)

            # Store in cache
            redis_client.setex(key, ttl_seconds, json.dumps(result))

            return result
        return wrapper
    return decorator

@cache(ttl_seconds=60)
def get_user(user_id):
    # Expensive database query
    return db.query(f"SELECT * FROM users WHERE id = {user_id}")
```

## Caching Best Practices

1. **Cache the right data**: Frequently accessed, expensive to compute, rarely changing
2. **Set appropriate TTLs**: Balance freshness vs. cache hit rate
3. **Handle cache failures gracefully**: The application should work without cache
4. **Monitor cache metrics**: Hit rate, memory usage, evictions
5. **Use consistent hashing**: For distributed caches to minimize cache misses during scaling

## Common Caching Layers

- **Browser Cache**: Static assets, API responses
- **CDN**: Static content, geographic distribution
- **Application Cache**: Redis, Memcached
- **Database Cache**: Query cache, buffer pool

---

## Diagrams in This Section

- [5.1-caching-strategies.drawio](./5.1-caching-strategies.drawio)
