#### Transmission Control Protocol (TCP)
![[Stateful & Stateless firewall.png]]
#### Stateful vs Stateless Firewalls
![[Stateful & Stateless firewall-1.png]]
**IMPORTANT**

Every “Connection” has two parts - **REQUEST** (initiation) and **RESPONSE**
![[Stateful & Stateless firewall-2.png]]
From the client *perspective*, this request is an outbound connection

So if you’re adding a firewall rule on the client, you would be looking to allow or deny an outbound connection

From the server perspective though, it’s an inbound connection.

The response is always the inverse direction to the request but the direction of the request isn’t always outbound and isn’t always inbound. It depends on what that data is, together with your *perspective*

##### Stateless firewall
![[Stateful & Stateless firewall-3.png]]

A stateless firewall means that it doesn’t understand the state of connections. What this means is that it sees the request connection from Bob’s laptop to catogram and the response, so from catogram to Bob’s laptop, as two individual parts

You need to think about allowing or denying them as two parts ⇒ You need two rules

In this case, one inbound rule, which is the request and one outbound rule for the response.

This is obviously more management overhead, two rules needed for each thing. Each thing which you as a human see as one connection.

Consider the situation where the catogram server is performing software updates. ⇒ The request will be from the catogram server to the software update server so outbound, and the response will be from the software update server to the catogram server so this is inbound

So this situation also requires two firewall rules, one outbound for the request and one inbound for the response.

There are two really important points

- For any servers where they accept connection and where they initiate connections. In this situation, you’ll have to deal with two rules for each of these and they will need to be the inverse of each other
- The request component is always going to be a well-know port. If you’re managing the firewall for the catgoram application, you’ll need to allow connections to TCP port 443. The response though, is always from the server to a client, but this always uses a random ephemeral port.

Because the firewall is stateless, it has no way of knowing which specific port is used for the response. So you’ll often have to *allow the full range* of ephemeral ports to any destination.

##### Stateful Firewalls
![[Stateful & Stateless firewall-4.png]]

A stateful firewall is intelligent enough to identify the response for a given request. Since the ports and IPs are the same, it can link one to the other.

This means that for a specific request to catogram from Bob’s laptop to the server, the firewall automatically knows which data is the response and the same is true for software updates

This means that with a stateful firewall, you’ll *generally only have to allow the request or not*, and the *response will be allowed or not automatically*.

This significantly reduces the admin overhead and the chance for mistakes

In addition, you *don’t need to allow the full ephemeral port range* because the firewall can identify which port is being used and implicitly allow it based on it being the response to a request that you allow.

[[Network Access Control Lists]]