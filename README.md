# Short-URL-Generation-System
 a high-performance URL shortening service using Spring Boot, Redis, and MySQL, handling over 1 million shortened URLs per day with efficient cache and collision prevention mechanisms.

**Architecture Overview**

**URL Shortening Workflow**

	1.	Client Request → A user submits a request to shorten a URL.
	2.	Shorten Service → Generates a unique shortened URL.
	3.	Redis Bloom Filter → Checks if the URL already exists in the system.
		If exists, returns the existing short URL.
		If new, proceeds to store the mapping.
	4.	Redis Cache → Temporarily stores the mapping for quick retrieval.
	5.	MySQL Storage → Persists the URL mapping for long-term storage.

**URL Resolution Workflow**

	1.	Client Request → A user submits a request to retrieve the original URL.
	2.	Shorten Service → Looks up the short URL.
	3.	Redis Cache → First checks Redis for the long URL.
	4.	MySQL Lookup → If not found in Redis, fetches from MySQL and updates Redis for future requests.

 **Tech Stack**
 
	•	Backend: Spring Boot (Java)
	•	Database: MySQL
	•	Cache: Redis
	•	Bloom Filter: Redis Bloom Filter (Redisson)
 	•       Containerization: Docker
  
**🚀 The Challenges:**

	• How to serve millions of requests while keeping latency low?
	• How to quickly check if a URL has been shortened before without overwhelming the database?

**🚀 The Solutions:**

	1.  Balancing Speed and Durability with a Tiered Storage Approach.
	Redis serves as the primary cache for quick lookups, reducing query latency for frequently accessed URLs.
 	MySQL acts as the persistent storage layer, ensuring long-term durability of the data.
  
	2.  Preventing Unnecessary DB Writes with Redis Bloom Filter. 
	When a new URL is submitted, the Redis Bloom Filter is checked before querying MySQL. 
 	If the Bloom Filter says “not seen before” → The system proceeds with normal processing (stores in Redis & MySQL). 
  	If the Bloom Filter says “might exist” → The system queries Redis first, then MySQL if necessary.
