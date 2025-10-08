**CDN** stands for **Content Delivery Network**.

It is a **network of servers** distributed across different geographic locations that work together to **deliver digital content (like websites, images, videos, etc.) to users quickly and efficiently**
![[CDN.png]]
## How Does a CDN Work?
When you visit a website, your browser downloads data (HTML, images, CSS, JavaScript, etc.) from a server. If that server is far from your location, the website can load **slowly**.
A **CDN solves this** by doing the following:
1. **Copies content** (like web pages, videos, images) from the original server (called the **origin server**).
2. **Stores it in multiple servers** (called **edge servers**) located around the world.
3. When a user accesses the content, the CDN delivers it from the **nearest edge server**, not the origin server
### Benefits of CDN
| Benefit                     | Explanation                                           |
| --------------------------- | ----------------------------------------------------- |
| **Faster load times**       | Content is delivered from servers close to the user.  |
| **Improved reliability**    | If one server goes down, another can take over.       |
| **Better scalability**      | Can handle large amounts of traffic efficiently.      |
| **DDoS protection**         | Many CDNs provide security features to block attacks. |
| **Reduced bandwidth costs** | Caching reduces the load on the origin server.        |
### Examples of Popular CDN Providers
- **Cloudflare**
- **Akamai**
- **Amazon CloudFront**
- **Google Cloud CDN**
- **Fastly**