### Question 3

Which is a benefit of caching?

* [ ] Increased application reliability
* [ ] Load balancing the application
* [x] Decreased costs
* [x] Reduced response latency

**Rationale:**
**Caching** improves performance by **storing frequently accessed data** closer to the user or application, reducing repeated computation or database queries. This leads to **lower latency** and **reduced operational costs** because fewer backend resources are consumed per request.

---

### Question 4

Which types of content on a web page can be cached using an edge cache? *(Select TWO.)*

* [x] Video files, such as a product demo
* [x] Web objects, such as hyperlinks
* [ ] Dynamically generated content, such as a user's name
* [ ] User-generated data, such as search terms entered by user
* [ ] Shopping cart filled with a user's items

**Rationale:**
**Edge caching** (for example, using Amazon CloudFront) is best suited for **static content**—files or objects that don’t change often—such as **videos, images, scripts, and web objects**. Dynamic or user-specific data (like personalized greetings, search inputs, or shopping carts) should not be cached because they vary per user session.

---

### Question 5

What does Amazon CloudFront enable?

* [ ] Transactional processing with an in-memory database
* [x] Multi-tiered and regional caching of content
* [ ] Automatic creation of a time to live (TTL) value
* [ ] Bidirectional caching between users and an origin host

**Rationale:**
**Amazon CloudFront** is a **content delivery network (CDN)** that enables **multi-tiered and regional caching** of static and dynamic content. It delivers data through a network of edge locations to reduce latency and improve performance for users worldwide. While CloudFront supports TTL settings, it does not create them automatically, and it is not used for transactional or bidirectional caching.


---

### Question 6

How does Amazon CloudFront use edge locations?

* [ ] It caches all content from an origin distribution at the edge location and delivers the content to clients through the fastest edge location.
* [ ] It caches local content at edge locations. It delivers the cached content to clients through the edge location that requires the fewest network hops to reach those clients.
* [x] It caches frequently accessed content at edge locations. It delivers the cached content to clients through the edge location with the lowest latency to those clients.
* [ ] It caches Regional data at Regional edge locations and delivers the content to clients through their Regional edge locations.

**Rationale:**
**Amazon CloudFront** caches **frequently accessed content** at its globally distributed **edge locations** to reduce latency and improve user experience. When a user requests content, CloudFront delivers it from the **edge location with the lowest latency**, ensuring faster data delivery. It does not cache all content or focus on network hops; it optimizes delivery based on latency and demand frequency.

---

### Question 7

Which statement describes an efficient way to deliver on-demand video content?

* [ ] Use Amazon S3 to store and serve the content.
* [ ] Launch an Amazon EC2 instance to host and serve your video content.
* [x] Use Amazon S3 to store the content. Then use Amazon CloudFront to deliver the content.
* [ ] Launch an Amazon EC2 instance to host your video content. Then use Amazon CloudFront to deliver the content.

**Rationale:**
The most **efficient and scalable** way to deliver **on-demand video content** is to use **Amazon S3** for storage and **Amazon CloudFront** as the **content delivery network (CDN)**. S3 provides durable, low-cost object storage, while CloudFront caches and distributes the content across global **edge locations**, reducing latency and minimizing load on the origin. Hosting and serving videos directly from EC2 is inefficient and costly compared to this architecture.

---

### Question 8

Which role does Amazon CloudFront play in protecting against distributed denial of service (DDoS) attacks?

* [ ] Controls traffic by the source IP addresses of requests
* [ ] Restricts traffic by geography to help block attacks that originate from specific countries
* [x] Routes traffic through edge locations
* [ ] Performs deep packet inspection to detect attacks

**Rationale:**
**Amazon CloudFront** helps protect applications against **DDoS attacks** by distributing incoming traffic across a large network of **edge locations**. This routing reduces the impact of high-volume attacks by absorbing and filtering malicious requests close to their source. CloudFront also integrates with **AWS Shield** and **AWS WAF** to enhance protection, but its primary DDoS defense mechanism lies in its globally distributed **edge infrastructure**.

---
### Question 9

How can an application use Amazon ElastiCache to improve database read performance? *(Select TWO.)*

* [ ] Replicate the database in ElastiCache, and direct all reads to ElastiCache and all writes to the database.
* [x] Read data from ElastiCache first, and write to ElastiCache when a cache miss occurs.
* [ ] Read data from the database first, and write the most frequently read data to ElastiCache.
* [x] Write data to ElastiCache whenever the application writes to the database.
* [ ] Direct all read requests to the database, and configure it to read from ElastiCache when a cache miss occurs.

**Rationale:**
**Amazon ElastiCache** improves database **read performance** by caching frequently accessed data in memory, reducing the load on the primary database. The two most common caching patterns are:

* **Cache-aside (lazy loading):** The application checks ElastiCache first; if data isn’t found (cache miss), it retrieves it from the database and then writes it to the cache for future reads.
* **Read-through:** The application reads from the database first and stores frequently accessed data in ElastiCache to accelerate subsequent queries.

---

### Question 10

Which type of caching strategy should be used when there's data that must be updated in real time?

* [ ] Lazy loading
* [ ] Cache-control headers
* [ ] Time to live (TTL)
* [x] **Write-through**

**Rationale:**
A **write-through caching** strategy updates the cache **at the same time** as the underlying database whenever data changes. This ensures the cache always holds the **most current data**, making it ideal for applications that require **real-time consistency** between the cache and the database. In contrast, lazy loading only updates the cache after a cache miss, which can result in stale data.
