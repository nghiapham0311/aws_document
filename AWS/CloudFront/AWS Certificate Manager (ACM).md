- HTTP - Simple and Insecure
- HTTP**S** - **SSL/TLS** Layer of Encryption added to HTTP
- Data is encrypted **in-transit**
- Certificates **prove identity**
- **Chain of trust** - Signed by a **trusted authority**
- ACM lets you run a **public** or **private** Certificate Authority (CA)
- **Private CA** - Applications need to **trust your private CA**
- **Public CA** - Browsers trust a list of providers, which can trust other providers

HTTP was initially created without the need for much in the way of security. So no server identity authentication or transport encryption. As HTTP evolved from just text to move towards complex web applications, the security vulnerabilities became an issue. For eg, if somebody could spoof a websites DNS name, then they could direct users to another potentially compromised web server. Users would be unaware of this because the address displayed by the browser would appear normal, just like they expect. And this could be used to gain access to credentials and potentially sniff data in transit.

The evolution of HTTP, HTTPS, was designed to address the problems with HTTP. It uses either SSL or TLS protocols to create a secure tunnel which normal HTTP can be transferred through. In effect, the data is encrypted in transit from the perspective of an outside observer.

HTTPS also allows for servers to prove their identity. Using SSL and TLS, servers can be authenticated by using digital Certificates. These can be digitally signed by one of the Certificate authorities trusted by the web client.

Since your web client trusts the CA, you trust the Certificate which it signs. That’s why you trust the site itself. It means that it’s harder to spoof. So the DNS name and the Certificate are tied together.
![[AWS Certificate Manager (ACM).png]]
If *you import* Certificates generated from another external source, then *you are going to be responsible for renewing* them with this external source and then importing them again into ACM.

The Certificates are always stored encrypted within the product, and deployed in a managed and secure way to those supported services. So services within AWS, which are *integrated with* *ACM*. **Not all services are supported**. This is generally **only CF and Load Balancers**. EC2 which is a self-managed compute service is not supported because AWS has no way of securing the transfer and deployment.
![[AWS Certificate Manager (ACM)-1.png]]

If you import a Certificate into a region or generate a Certificate in that region, those Certificates cannot leave that region. Once inside, they’re locked to that particular region.

To use a Certificate from ACM inside a load balancer within a particular region, that Certificate needs to be within ACM also in that region.

For CF, that service, while being global, you should view it as running in `us-east-1` from an ACM perspective. That a distribution, which is the unit of configuration for CF is actually within `us-east-1`. So you always use `us-east-1` for CF certificates.

![[AWS Certificate Manager (ACM)-2.png]]

Step one is that our security specialist will interact with ACM in each of these regions, and generate a Certificate, and then deploy it out to ACM in each region where service is required. This means for supported services in those regions, such as application load balancers, those Certificates can be used from ACM to services in those regions.

What we can’t do is deploy cross region. Cross region deployment is not supported. Nor can we deploy to unsupported services such as EC2.

Once the Certificate is linked to the distribution, the distribution can then take the Certificate and deploy it out to the edge locations no matter what regions they’re located in. Once the Certificates are deployed onto supported services they can be used. They allow trust to be established from customers to the application load balancer, as with the top example.

Once the ACM certificate is deployed to the distribution, then passed out to the edge locations, customers can make secure connections to those edge locations.

ACM isn’t used for S3, which handles its own interaction with CF within this architecture. So S3 does not use ACM for any certificates.

[[CloudFront - Security - OAI & Custom Origins]]