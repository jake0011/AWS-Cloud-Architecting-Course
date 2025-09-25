# Module 12: Caching Content
This module introduces **Amazon CloudFront** and **Amazon ElastiCache**, two AWS services for caching data to improve application performance and reduce latency. It covers the fundamentals of caching, how to use CloudFront for content delivery and ElastiCache for database caching, and how to apply **AWS Well-Architected Framework** principles to caching strategies. The module includes a guided lab and knowledge checks, with a focus on the fictional café scenario for practical application.

## Introduction
  - This section outlines the module’s content, emphasizing the role of caching in optimizing application performance, reducing latency, and balancing cost with data freshness.

- **Module Objectives**:
  - Identify how **caching content** improves application performance and reduces latency.
  - Use **Amazon CloudFront** to deliver content via edge locations with protection.
  - Create architectures that leverage CloudFront for content caching.
  - Describe how to use **Amazon ElastiCache** for database caching.
  - Apply **AWS Well-Architected Framework principles** to design effective caching strategies.

- **Module Overview**:
  - **Presentation Sections**:
    - Overview of caching
    - Caching using CloudFront
    - Caching using ElastiCache
    - Applying AWS Well-Architected Framework principles to caching
  - **Knowledge Checks**:
    - 10-question knowledge check (delivered in the online course)
    - Sample exam question for class discussion

- **Hands-on Labs**:
  - **Guided Lab**:
    - **Streaming Dynamic Content Using Amazon CloudFront**: Step-by-step instructions to deploy a CloudFront distribution for content delivery.
    - Additional details in the student guide; lab environment provides detailed instructions.

- **Cloud Architect Considerations**:
  - Determine when and how to cache content to balance **performance**, **cost**, and **data freshness** to meet business requirements.
  - Incorporate a **content delivery network (CDN)** like CloudFront to distribute static content securely and efficiently.
  - Select an **in-memory caching strategy** (e.g., ElastiCache) to reduce network latency and offload database pressure.
  - Work backward from business needs (e.g., the café scenario) to design caching architectures tailored to specific use cases.

---

## Section 2: Caching Using CloudFront: Caching Content
This section explores how **Amazon CloudFront**, a content delivery network (CDN) service, facilitates **static caching** to deliver content securely with low latency and high transfer speeds. It covers the role of CDNs, cacheable content types, CloudFront’s global edge network, caching mechanisms, configuration steps, video streaming capabilities, and DDoS mitigation features.

- **Content Delivery Network (CDN) Delivery Challenge and Solution**:
  - **Challenge**:
    - Delivering content to geographically dispersed users introduces latency due to large physical distances.
    - Two-way communication (client requests and server responses) can be slow for large files (e.g., videos, images) if the server is far from the user.
  - **CDN Solution**:
    - A CDN is a **globally distributed system of caching servers** that stores static content closer to users.
    - **Intermediary servers** (edge locations) between the client and the application cache commonly requested files (e.g., HTML, CSS, JavaScript, images, videos).
    - Delivers a **local copy** of content from a nearby **cache edge** or **point of presence (PoP)**, reducing latency and bandwidth consumption.
    - Benefits: Decreases web traffic to the origin server, improves user experience, and enhances application efficiency.

- **Cacheable Content with CloudFront**:
  - **Cacheable Content**:
    - **Images**, **videos**, and **web objects** (e.g., HTML, CSS, JavaScript files) can be cached at edge locations.
    - Example: Static assets on an Amazon.com webpage (e.g., product images, CSS stylesheets).
  - **Non-Cacheable Content**:
    - **Dynamically generated content** (e.g., personalized search results) and **user-generated data** cannot be cached.
    - CloudFront can deliver dynamic content from a **custom origin** (e.g., EC2 instance, web server) but does not cache it.
  - **Security**:
    - CloudFront supports **HTTPS** for encrypted communication between viewers and CloudFront, and between CloudFront and the origin.
    - Ensures secure delivery of both static and dynamic content.

- **CloudFront Overview**:
  - **Definition**: A CDN service that delivers content globally with **low latency** and **high transfer speeds**, built for performance, security, and developer convenience.
  - **Key Features**:
    - Uses a **global network of edge locations** for high-speed content distribution.
    - Routes user requests through the **AWS backbone network** to the nearest edge location for optimal performance.
    - Enhances **application resiliency** against **distributed denial of service (DDoS)** attacks using **AWS Shield** and **AWS WAF**.
    - Supports **HTTPS** with the latest TLS version for secure communication.
  - **Benefits**:
    - Reduces latency by caching content at edge locations.
    - Increases reliability and availability with multiple cached copies worldwide.
    - Protects against network and application layer attacks.

- **CloudFront Global Edge Network Components**:
  - **Edge Locations**:
    - Over **550 edge locations** in more than **100 cities across 50 countries** (as of October 2023).
    - Closer to users, with smaller caches for **popular content**.
    - Ensures fast delivery of frequently accessed files.
  - **Regional Edge Caches**:
    - Fewer locations (13 globally), farther from users, with **larger caches**.
    - Stores **less popular content** (e.g., user-generated content, ecommerce assets, news/event content) for longer periods.
    - Reduces the need to fetch from the origin server, improving performance.
  - **AWS Backbone Network**:
    - Connects edge locations to AWS Regions via **redundant, high-speed 100 GbE fibers**.
    - Enhances origin fetches and dynamic content acceleration.
  - **Resources**: See the “Amazon CloudFront Key Features” page in course resources for edge location details.

- **How Caching Works in CloudFront**:
  - **Process**:
    1. A user requests content (e.g., `cat.jpg`).
    2. **DNS** routes the request to the nearest **edge location** with the shortest latency.
    3. CloudFront checks the edge cache:
       - If the content is cached, it is delivered immediately (step 5).
       - If not cached, CloudFront forwards the request to the **origin** (e.g., an S3 bucket).
    4. The origin sends the content to the edge location.
    5. CloudFront delivers the content to the user and caches it for future requests.
  - **Time to Live (TTL)**:
    - Determines how long content remains cached before CloudFront checks the origin for updates.
    - Default maximum TTL is **24 hours**; cached content becomes stale if the origin updates.

- **How to Configure a CloudFront Distribution**:
  - **Steps**:
    1. **Specify an Origin Location**:
       - Define the source of content (e.g., **S3 bucket**, **HTTP server** on EC2, or an **on-premises server**).
    2. **Create a CloudFront Distribution**:
       - Specify the origin and settings (e.g., logging, enabling/disabling the distribution).
    3. **CloudFront Becomes Available**:
       - Assigns a domain name to the distribution.
    4. **Distribute Configuration**:
       - CloudFront sends the distribution’s configuration (not content) to all edge locations for caching.
  - **Use Case**: Configure an S3 bucket as the origin to serve static website content globally via CloudFront.

- **Controlling How Long Content is Cached**:
  - **Methods**:
    - **Set a Maximum TTL Value**:
      - Content expires between the minimum and maximum TTL (default max: 24 hours).
      - Lowering the max TTL forces faster expiration, increasing origin requests but ensuring fresher content.
    - **Implement Content Versioning**:
      - Embed version identifiers (e.g., timestamps, sequential numbers) in file names.
      - Bypasses TTL by forcing CloudFront to fetch new files immediately (e.g., `image-v2.jpg` vs. `image-v1.jpg`).
    - **Specify Cache-Control Headers**:
      - Use HTTP `Cache-Control: max-age` headers for individual files to control expiration.
      - With a minimum TTL of 0, CloudFront honors these headers for precise control.
    - **Use CloudFront Invalidation Requests**:
      - Force CloudFront to expire specific content from edge caches.
      - Takes several minutes to process; use sparingly for individual objects.
      - CloudFront fetches the latest version from the origin on the next request.
  - **Best Practice**: Combine these methods to balance performance and data freshness.
  - **Troubleshooting Tip**: If stale content persists, check TTL settings or use invalidation requests.

- **Using CloudFront for Video Streaming**:
  - **Process**:
    1. Use an encoder (e.g., **AWS Elemental MediaConvert**, **Amazon Elastic Transcoder**) to format and package video content.
    2. Host the converted content in an **S3 bucket** (origin server).
    3. CloudFront distributes **segment files** and **manifest files** to users globally.
  - **Packaging Formats**:
    - **Dynamic Adaptive Streaming over HTTP (DASH/MPEG-DASH)**, **Apple HTTP Live Streaming (HLS)**, **Microsoft Smooth Streaming**, **Common Media Application Format (CMAF)**.
    - Segments are static files containing audio, video, and captions; manifests define playback order.
  - **Use Case**: Deliver streaming videos to global users with low latency via CloudFront edge locations.

- **DDoS Mitigation with CloudFront**:
  - **Security Challenge**:
    - Publicly accessible content is vulnerable to threats like **OWASP Top 10 vulnerabilities**, **SQL injection**, **automated requests**, and **HTTP floods** (DoS attacks).
    - DDoS attacks aim to disrupt availability by overwhelming servers with traffic from multiple sources (e.g., malware-infected devices, IoT endpoints).
  - **CloudFront Solution**:
    - **Amazon Route 53**:
      - Routes traffic to CloudFront with built-in **always-on monitoring**, **anomaly detection**, and **mitigation** for infrastructure DDoS attacks.
    - **AWS WAF (Web Application Firewall)**:
      - Analyzes incoming requests and blocks threats (e.g., SQL injection, cross-site scripting) via a **web access control list (ACL)**.
      - Configures rules to protect servers and attaches the ACL to the CloudFront distribution.
    - **AWS Shield**:
      - Provides **automatic DDoS mitigation** for CloudFront, filtering out invalid traffic (e.g., UDP reflection attacks).
      - Ensures only valid web application traffic reaches the origin.
  - **Benefits**:
    - Protects both static and dynamic content.
    - Enhances resiliency against network and application layer attacks.
    - Supports secure delivery with HTTPS and access control methods.

- **Key Takeaways: Caching Using CloudFront**:
  - CloudFront enables **static file caching** via a CDN, using **edge locations** and **Regional edge caches** to deliver content globally with low latency.
  - Controls caching behavior with **TTL settings**, **content versioning**, **cache-control headers**, and **invalidation requests**.
  - Supports **video streaming** by distributing packaged content from S3 buckets.
  - Enhances security with **AWS Shield**, **AWS WAF**, and **Route 53** for DDoS mitigation and threat protection.

---

## Section 3: Caching Using ElastiCache: Caching Content
This section explains how **Amazon ElastiCache** facilitates **database caching** to improve application performance by reducing latency and database load. It covers when to use ElastiCache, the differences between **Memcached** and **Redis** engines, ElastiCache components, time to live (TTL) settings, and caching strategies (lazy loading and write-through) to manage stale data.

- **When to Cache Your Database**:
  - **Use Cases**:
    - **Concern About Response Times**: Cache to reduce latency for latency-sensitive workloads, improving application performance and throughput.
    - **High Volume of Requests**: Offload excessive read traffic from the database to increase throughput and handle large-scale workloads.
    - **Reduce Database Costs**: Minimize the need for costly database scaling (e.g., read replicas) by using a single in-memory cache node to handle high read requests.
  - **Benefits**:
    - Supplements the primary database by caching frequently accessed read data.
    - Reduces bottlenecks from time-consuming or complex database queries.
  - **Example Use Case**:
    - An **online medical library** with a search engine overloaded by high query volumes, saturating low-speed network links.
    - **Solution**: Use ElastiCache as an in-memory cache to reduce network latency and offload database pressure.
  - **Considerations**:
    - Caching improves performance but requires strategies to manage **stale data** to avoid incorrect system behavior.

- **Amazon ElastiCache Overview**:
  - **Definition**: A managed, in-memory key-value data store/cache service supporting **Redis** and **Memcached** engines.
  - **Purpose**: Enhances web application performance by retrieving data from fast, in-memory stores instead of slower disk-based databases.
  - **Compatibility**: Supports existing Redis and Memcached application code, drivers, and tools, enabling seamless deployment and scaling.
  - **Use Cases**:
    - **ElastiCache for Redis**: Manage and analyze fast-moving data with advanced features.
    - **ElastiCache for Memcached**: Build scalable caching tiers for data-intensive applications.

- **Choosing an ElastiCache Engine**:
  - **ElastiCache for Memcached**:
    - **Features**:
      - **Low Maintenance**: Simplest model, requiring minimal administrative overhead.
      - **Multithreading**: Supports multiple cores/threads for high compute capacity.
      - **Horizontal Scalability with Auto Discovery**: Automatically detects node changes (add/remove) in clusters, reconfiguring clients dynamically.
    - **Best For**:
      - Basic caching needs.
      - Large nodes with multiple cores/threads.
      - Workloads requiring horizontal scaling (up to 20 nodes).
  - **ElastiCache for Redis**:
    - **Features**:
      - **Advanced Data Structures**: Supports complex types (strings, hashes, lists, sets, sorted sets, bitmaps).
      - **Persistence**: Maintains data durability, unlike Memcached.
      - **Sorting/Ranking**: Enables sorting or ranking in-memory datasets (e.g., game leaderboards).
      - **High Availability**: Supports Multi-AZ deployments with automatic failover.
      - **Publish/Subscribe Messaging**: Enables real-time features like chat rooms, comment streams, or social media feeds.
    - **Best For**:
      - Complex data types, persistent storage, or high-availability requirements.
      - Workloads needing up to 250 nodes.
  - **Performance**: Both engines offer **sub-millisecond response times** for in-memory data access.

- **ElastiCache Components**:
  - **Node**: The smallest building block, a fixed-size chunk of secure, network-attached RAM running the chosen engine (Redis or Memcached).
    - Each node has a unique **DNS name** and **port**.
    - Can operate in isolation or within a cluster.
    - Scalable by changing instance types (scale up/down).
  - **Cluster**:
    - A logical grouping of one or more nodes.
    - **Memcached**: Data is partitioned across up to **20 nodes** for scalability.
    - **Redis**: Supports up to **250 nodes**, organized into **shards** (1-6 nodes per shard) with Multi-AZ high availability.
  - **Endpoint**: The unique address used by applications to connect to a node or cluster.

- **Time to Live (TTL)**:
  - **Definition**: An integer value specifying the duration (in seconds or milliseconds, depending on the engine) until a cached key expires.
  - **Behavior**:
    - When a key expires, the application treats it as missing, querying the primary database and updating the cache.
    - Ensures data does not become overly stale and is periodically refreshed from the database.
  - **Importance**: Critical for managing cache freshness and aligning with business requirements for data accuracy.

- **Caching Strategies for Handling Stale Data**:
  - **Lazy Loading**:
    - **Mechanism**: Loads data into the cache only when requested (cache miss).
    - **Process**:
      1. User requests content.
      2a. If data is in the cache (cache hit), ElastiCache returns it immediately.
      2b. If data is not in the cache (cache miss), the request goes to the primary database, which returns the data and updates the cache.
    - **Advantages**:
      - Caches only requested data, avoiding unnecessary storage.
      - Reduces cache size and costs.
    - **Disadvantages**:
      - Cache misses cause delays (three trips: application to cache, cache to database, database to application).
      - Data may become stale if not updated proactively.
    - **Best For**: Data read often but written infrequently (e.g., user profiles accessed frequently but rarely updated).
  - **Write-Through**:
    - **Mechanism**: Updates the cache simultaneously with every database write.
    - **Process**:
      1. Application updates the database.
      2. Cache is updated immediately to reflect the change.
    - **Advantages**:
      - Ensures cache is always up-to-date, minimizing cache misses and stale data.
      - Improves performance for frequently accessed data.
    - **Disadvantages**:
      - Increases costs by caching potentially unneeded data.
      - Requires more storage for the cache.
    - **Best For**: Data requiring real-time updates (e.g., inventory or pricing data).
  - **Considerations**:
    - **Business Impact**: Choose write-through for high-impact stale data scenarios; lazy loading for low-impact scenarios.
    - **Data Change Frequency**: Frequent updates favor write-through; infrequent updates suit lazy loading.
    - **Implementation**: Requires application-level logic to manage the chosen strategy, balancing cost, speed, and data freshness.

- **Key Takeaways: Caching Using ElastiCache**:
  - ElastiCache is a **key-value, in-memory data store** providing **millisecond latency** and cost-effective data access.
  - Supports **Redis** and **Memcached** engines, reducing complexity in administering caching systems.
  - **Lazy loading** and **write-through** strategies manage stale data, with trade-offs in performance, cost, and freshness based on business needs.

---

# Applying AWS Well-Architected Framework Principles to Caching: Caching Content
This section explores how to apply **AWS Well-Architected Framework** principles to caching strategies, focusing on the **Performance Efficiency**, **Reliability**, and **Cost Optimization** pillars. It provides best practices for using **Amazon CloudFront** and **Amazon ElastiCache** to optimize performance, ensure high availability, and reduce costs while managing data freshness.

- **AWS Well-Architected Framework Caching Best Practices**:
  - The AWS Well-Architected Framework includes six pillars: Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, and Sustainability.
  - This section highlights caching-related best practices from the **Performance Efficiency**, **Reliability**, and **Cost Optimization** pillars.
  - **Cloud Architect Considerations**:
    - Determine **when to cache content** to improve performance and optimize costs.
    - Address **stale content** to balance data freshness with cost management.
    - Incorporate **CDN services** (e.g., CloudFront) and **in-memory caching** (e.g., ElastiCache) into architecture designs.
  - For complete best practices, see the Well-Architected Framework website (linked in course resources).

- **Best Practice Approach: Data Management (Performance Efficiency Pillar)**:
  - **Objective**: Improve read latency, throughput, user experience, and overall efficiency while reducing costs by caching data.
  - **Best Practices**:
    - **Implement Data Access Patterns That Utilize Caching**:
      - **Identify Candidates**: Focus on databases, APIs, or network services with **heavy read workloads**, **high read-to-write ratios**, or **expensive scaling** needs.
      - **Select Caching Strategy**: Choose the appropriate caching type (e.g., static caching with CloudFront, database caching with ElastiCache) based on access patterns.
      - **Configure Cache Invalidation**: Use **time-to-live (TTL)** settings to balance data freshness and reduce pressure on backend datastores.
      - **Monitor Cache Hit Rate**: Aim for a cache hit rate of **80% or higher**; lower rates may indicate insufficient cache size or unsuitable access patterns.
      - **CloudFront Example**: Control caching with **minimum/maximum TTL**, **content versioning**, **cache-control headers**, and **invalidation requests** to manage stale content.
      - **ElastiCache Example**: Use **lazy loading** or **write-through** strategies to optimize cache updates and ensure data accuracy.
    - **Implement Strategies to Improve Query Performance in Data Store**:
      - Optimize data and query performance to enhance efficiency and reduce resource usage.
      - Use **distributed caching** (e.g., ElastiCache) to improve latency and reduce database I/O operations.
      - **ElastiCache Benefits**: Provides **microsecond latency** for real-time applications (e.g., session management, gaming leaderboards, geospatial apps) compared to millisecond latency of disk-based databases.
      - Unoptimized queries increase resource usage and create bottlenecks, impacting overall workload performance.
  - **Supporting AWS Services**:
    - **Amazon CloudFront**: Caches static content at edge locations to reduce latency.
    - **Amazon ElastiCache**: Provides in-memory caching for databases, supporting Redis and Memcached for low-latency data access.
  - **Resources**: See the “Performance Efficiency Pillar” whitepaper in course resources.

- **Best Practice Approach: Foundations – Plan Your Network Topology (Reliability Pillar)**:
  - **Objective**: Ensure high availability and reliability for public endpoints through resilient network connectivity.
  - **Best Practices**:
    - **Use Highly Available Network Connectivity for Public Endpoints**:
      - Plan and operationalize network connectivity to prevent workload unavailability due to connectivity loss.
      - Use **CloudFront** to distribute content via a global network of **edge locations** and **Regional edge caches**, shifting load from origin servers.
      - Benefits:
        - Improves **reachability** and **availability** by caching content closer to users.
        - Enhances **resiliency** against DDoS attacks with **AWS Shield** and **AWS WAF**.
      - Example: CloudFront routes requests through the AWS backbone network to the nearest edge location, ensuring reliable content delivery.
  - **Supporting AWS Services**:
    - **Amazon CloudFront**: Provides low-latency, high-availability content delivery.
    - **Amazon Route 53**: Routes traffic to CloudFront with built-in monitoring and DDoS mitigation.
    - **AWS Shield** and **AWS WAF**: Protect against network and application-layer attacks.
  - **Resources**: See the “Reliability Pillar” whitepaper in course resources.

- **Best Practice Approach: Cost-Effective Resources – Plan for Data Transfer (Cost Optimization Pillar)**:
  - **Objective**: Minimize data transfer costs by selecting appropriate services and configurations for caching workloads.
  - **Best Practices**:
    - **Perform Data Transfer Modeling**:
      - Analyze workload components to identify data transfer costs and benefits.
      - Model data transfer requirements to find the lowest-cost architecture.
      - Example: Assess whether CloudFront edge caching reduces origin server requests enough to justify costs.
    - **Select Components to Optimize Data Transfer Cost**:
      - Identify high-cost data transfer points or potential cost increases with workload growth.
      - Use **CDNs** (e.g., CloudFront) or alternative architectures to reduce data transfer needs.
      - Example: Cache static content with CloudFront to minimize origin server bandwidth usage.
    - **Implement Services to Reduce Data Transfer Costs**:
      - Use **CloudFront** to cache content at edge locations, reducing origin server load and administrative effort.
      - Build **caching layers** (e.g., ElastiCache) in front of application servers or databases to offload traffic.
      - Leverage services for **compression**, **caching**, and **traffic distribution** to optimize costs.
      - **CloudFront Cost Savings**: The **security savings bundle** can save up to 30% on CloudFront usage for growing workloads.
      - Example: Control CloudFront caching with **TTL settings**, **content versioning**, **cache-control headers**, and **invalidation requests** to optimize cache duration and reduce origin fetches.
  - **Supporting AWS Services**:
    - **Amazon CloudFront**: Reduces data transfer costs by caching content globally.
    - **Amazon ElastiCache**: Lowers database query costs by caching frequently accessed data.
  - **Resources**: See the “Cost Optimization Pillar” whitepaper in course resources.

- **Key Takeaways: Applying AWS Well-Architected Framework Principles to Caching**:
  - **Performance Efficiency Pillar**:
    - Select caching strategies (e.g., CloudFront for static content, ElastiCache for database caching) based on data characteristics and access patterns.
    - Optimize query performance with distributed caching to reduce latency and database load.
  - **Reliability Pillar**:
    - Use CloudFront’s highly available edge network to ensure reliable content delivery and protect against DDoS attacks.
  - **Cost Optimization Pillar**:
    - Implement CloudFront and ElastiCache to reduce data transfer and database costs, modeling workloads to optimize resource usage.

---

### Module summary:
This module prepared you to do the following: 
- Identify how caching content can improve application performance and reduce latency.
- Identify how to use Amazon CloudFront to deliver content by using edge locations protection.
- Create architectures that use CloudFront to cache content.
- Describe how to use ElastiCachefor database caching.
- Use the AWS Well-Architected Framework principles when designing caching strategies.
