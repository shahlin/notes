# Proxy / Forward Proxy
- A forward proxy, often called a proxy, proxy server, or web proxy, is a server that sits in front of a group of client machines
- When a client/computer makes a request to a website or services on the internet, the proxy server intercepts this request and redirects the request to the web server
- The response from the web server is returned back to the proxy and the proxy in turn responds to the client
- It acts like a middleman between client and internet

![forward_proxy_flow](https://github.com/user-attachments/assets/9343b5cd-a2fd-4761-8282-447a66eae493)

## Uses
1. To avoid any kind of browsing restrictions (bypassing them)
2. To block access to certain content
3. To protect their identity online (hide client identity like IP)

# Reverse Proxy
- A reverse proxy is a server that sits in front of one or more web servers, intercepting requests from clients.
- When a client/computer makes a request to a website or services on the internet, the request is sent to the internet wherein the reverse proxy intercepts the call and redirects it to the web server
- It acts as a middleman between internet and web  servers
- The difference between a forward and reverse proxy is subtle but important. A reverse proxy sits in front of an origin server and ensures that no client ever communicates directly with that origin server

![reverse_proxy_flow](https://github.com/user-attachments/assets/af7ffdee-4838-4132-a2b3-354468b79e6e)

## Uses
1. Load Balancing
2. Protection from attacks
3. Caching
4. SSL Encryption

# References
- https://www.cloudflare.com/learning/cdn/glossary/reverse-proxy/
