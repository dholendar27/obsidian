---
date created: 2025-09-22 17:29
---

The Algorithms or techniques that decide which data should be deleted from a cache when the capacity is reached is known as Cache Eviction Policies

## Least Recently Used

When a cache reaches its capacity limit, the Least Recently Used (LRU) eviction policy removes the item that has been accessed the least recently ( The data that has not been accessed for the long time). It assumes that items is not used for a longer time are less likely to be needed . When the cache is full, LRU evicts the item that hasn’t been accessed for the longest period, as it tracks the order in which items are accessed.
![[LRU.png]]
### Advantages of Least Recently Used (LRU)

1. **Simple Implementation**: LRU is easy to understand and implement, making it a popular choice for caching systems across various applications.
2. **Efficient Cache Utilization**: In scenarios where recent access patterns are a good predictor of future access, LRU effectively ensures that frequently accessed items stay in the cache, optimizing performance.
3. **Versatile**: LRU adapts well to different environments, including web caching, databases, and file systems, making it a flexible solution for various types of applications.
### Disadvantages of Least Recently Used (LRU)

1. **Strict Access Order**: LRU assumes that the most recently accessed items will continue to be relevant in the future. However, this assumption doesn't always hold, leading to less optimal cache management in some cases.
2. **Cold Start Problem**: LRU may not perform well when a cache is first populated, as it requires historical data to make accurate eviction decisions, which isn't available initially.
3. **Additional Memory Requirements**: Implementing LRU typically involves maintaining timestamps or an access order list, which can increase memory consumption and potentially affect overall system efficiency.

### Use Cases for Least Recently Used (LRU)

1. **Web Caching**: LRU is widely used to store frequently accessed web pages, images, or other resources, improving response times and reducing latency by keeping popular content ready for quick retrieval.
2. **Database Management**: In databases, LRU is employed to cache frequently queried data or database pages, which reduces the need to fetch data from slower disk storage, speeding up query response times.
3. **File Systems**: File systems can leverage LRU to cache metadata or directory information, ensuring that frequently accessed files and directories are readily available, which improves access speed and reduces disk load.
## Least Frequently used
The LFU is based on the idea where the least frequently used things are less likely to be needed later, when the cache is full, LFU removes the item with the lower access frequency after keeping track of the amount of times each item is accessed. Every item in the cache has an associated **frequency count**.
### Advantages of Least Frequently Used (LFU)
1. **Adapts to Different Access Patterns**: LFU works well in situations where some items are used less often but are still important. It adjusts to changes in how often things are accessed.
2. **Focuses on Long-Term Trends**: LFU is good at capturing long-term usage patterns. It cares more about how often something is accessed over time than just recent activity.
3. **Uses Less Memory**: Unlike some other methods, LFU doesn't need to store time stamps, so it can be more memory-efficient.

### Disadvantages of Least Frequently Used (LFU)

1. **Sensitive to Initial Access**: LFU might not work well at first when access patterns aren’t clear yet. It takes time for it to figure out what’s important, and new or rarely used items may not be kept in the cache until they’ve been used enough.
2. **Struggles with Changing Access Patterns**: LFU can have problems if access patterns change quickly. Items that were popular in the past but are no longer relevant might stay in the cache too long.
3. **Complexity with Counting Frequencies**: Counting how often things are used adds some complexity to LFU, which can make it harder to implement.

### Use Cases of Least Frequently Used (LFU)

1. **Database Query Caching**: LFU can be used to store query results or data that’s accessed often in databases, so it doesn’t have to be fetched repeatedly.
2. **Network Routing**: LFU helps cache routing information in networks, keeping less frequently used routes in memory for quicker decision-making.
3. **Content Recommendations**: LFU can be used in recommendation systems to remember user preferences or content that’s been accessed, ensuring even less popular items are considered over time.
## First-In-First-Out (FIFO)
This strategies uses the first in first out method to remove the old data that is added into the cache and the data that has been present in the cache for the longest of time.
### Advantages of First-In-First-Out (FIFO)

1. **Simple to Implement**: FIFO is easy to understand and implement, making it a good choice when simplicity and straightforwardness are important.
2. **Predictable Behavior**: The eviction process follows a clear rule—items are removed in the order they were added. This predictability can be useful in certain applications where the sequence of actions matters.
3. **Memory Efficient**: FIFO doesn’t require tracking access frequency or timestamps, so it uses minimal memory overhead compared to some other cache eviction strategies.

### Disadvantages of First-In-First-Out (FIFO)

1. **Lack of Adaptability**: FIFO doesn’t adjust to changes in how often items are used. It removes items based purely on their entry time, not on how relevant or frequently accessed they are.
2. **Inefficiency with Varying Importance**: FIFO can be inefficient when newer items are more important or accessed frequently. Older items might still stay in the cache just because they were added first, even if they’re no longer relevant.
3. **Cold Start Problems**: After a cache is cleared or when it’s first populated, FIFO can perform poorly because it doesn’t consider the actual usage of the items—it just keeps the oldest items, regardless of how useful they are.
    

### Use Cases of First-In-First-Out (FIFO)

1. **Task Scheduling in Operating Systems**: FIFO is commonly used in task scheduling, where processes or tasks are executed in the order they are added, ensuring fairness and simplicity.
2. **Message Queues**: FIFO guarantees that messages are processed in the same order they are received, which is essential for message-based systems where maintaining the order of communication is critical.
3. **Cache for Streaming Applications**: In some streaming apps, such as video streaming, where the order of frames or data matters, FIFO ensures that the data is handled in the correct sequence.