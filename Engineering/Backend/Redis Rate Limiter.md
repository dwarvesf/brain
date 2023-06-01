## Introduction
There are many use cases for an in-memory NoSQL database, such as Redis. One particular case that happens in enterprise applications is creating rate limits for labeled data sets. Below is a demonstration of how to set up basic rate limiting on Redis.

## Data types in Redis for Rate Limiting
The idea is to have a data label such that it labels exactly which user is accessing a resource at any given time. The easiest case for us is to use the user’s IP address. We can hold their IP address as a key on Redis or a sub-item on any one of Redis’ data types.

Two of the data types we will cover will be a simple key-value pair with a counter, and a LIST, both of which will have expiration dates to rate limit by a certain period of time.

# case 1: use counter

![](https://i.imgur.com/gWWpbQl.png)
One simple implementation using LUA on Redis would be to use a response counter to check how many requests there are within a timespan. Below is an example of a counter with an expiry date of 10 seconds, with the idea that the counter will rate limit the data label 10 times every 10 seconds. This implementation is susceptible to race conditions:

```
keyname = ip+":"+ts
MULTI
    INCR(keyname)
    EXPIRE(keyname,10)
EXEC
current = RESPONSE_OF_INCR_WITHIN_MULTI
IF current > 10 THEN
    ERROR "too many requests per second"
ELSE
    PERFORM_API_CALL()
END
```

# case 2: use list
![](https://i.imgur.com/RkMMJtW.png)
Another implementation using LUA on Redis would be to use a LIST with an expiration time. Using LISTs here can help us avoid race conditions as RPUSHX helps only to push IPs if it exists on the list. On the other hand, this method can easily raise errors for cases where there are no IPs.
```
current = LLEN(ip)
IF current > 10 THEN
    ERROR "too many requests per second"
ELSE
    IF EXISTS(ip) == FALSE
        MULTI
            RPUSH(ip,ip)
            EXPIRE(ip,1)
        EXEC
    ELSE
        RPUSHX(ip,ip)
    END
    PERFORM_API_CALL()
END
```

## Conclusion
Above are 2 simple examples exploring the usefulness of data types to help handle rate limiting on Redis. The first case is the most simplest, but is susceptible to race conditions. On the other hand, the second case is a lot more elegant in that it removes the issue of race conditions, but may have errors raised during edge cases such as missing an API call.

## Reference
- https://redis.com/glossary/rate-limiting/
- https://redis.io/docs/data-types/lists/