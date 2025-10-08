The Domain Name System (DNS) is the phonebook of the internet. we access the information or the website using the domain names. but the client(browser) doesn't know where the website that we are hoisted and the ip-address of that. so we use DNS to lookup for the website that we are checking and provide the ip address
![[DNS Explanation.png]]
## How DNS Works
1. **You enter** `www.google.com` into your browser.
2. **Browser checks the cache** (also called a stub resolver):
    - If you’ve visited recently, the IP might be cached locally.
    - If found, it uses the cached IP address to load the website.
3. **If not found in the browser cache**, the browser asks the **DNS resolver** (usually from your ISP, assigned via DHCP, or manually set like Google DNS `8.8.8.8`).
4. **The DNS resolver** (called a recursive resolver) checks:
    - Its own cache (has it resolved this recently for anyone?).
    - If not cached, it **starts a recursive query** to find the IP.
5. **The DNS resolver contacts the Root DNS Servers**:
    - The **Root DNS server** doesn’t know the IP of `www.google.com`.
    - But it **knows where to find the `.com` TLD servers**.
    - It responds: “Ask the `.com` TLD server — here’s its IP.”
6. **The resolver then contacts the `.com` TLD server**:
    - The TLD server doesn’t know the IP either.
    - But it knows where the **authoritative name server** for `google.com` is.
    - It replies: “Ask Google’s name server — here’s its IP.”
7. **The resolver contacts Google’s authoritative DNS server**:
    - This server **knows the exact IP** of `www.google.com`.
    - It responds with the IP address (e.g., `142.250.190.4`).
8. **The resolver returns the IP** to your browser.
9. **Your browser connects** to the IP address using HTTP(S) and loads the site.
10. **Result is cached**:
- The IP is cached in your system (stub), resolver, and maybe browser for faster access next time.