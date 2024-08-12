A network access control list (ACL) is an *optional layer of security* for your VPC that acts as a *firewall* for *controlling traffic in and out of one or more subnets*. You might set up network ACLs with rules similar to your security groups in order to add an additional layer of security to your VPC

![[Network Access Control Lists.png]]

Every subnet has an associated network ACL and this filters data as it crosses the boundary of that subnet

In practice this means any data coming into the subnet is affected and data leaving the subnet is affected. But the connections between things within that subnet, such as between instance A and instance B in this example, are not affected by network ACLs

Each network ACL contains a number of rules, *two sets of rules* to be precise.

We have *inbound rules* and *outbound rules*

- Inbound rules only affect data entering the subnet
- Outbound rules affect data leaving the subnet

NACLs are *stateless*, which means they don’t know if traffic is request or response

Network ACLs offer both explicit allows and explicit denies

Rules are processed in order:

1. A network ACL determines if the inbound or outbound rules apply
2. It starts from the lowest rule number. It evaluates traffic against each individual rule until it finds a match
3. The traffic is either allowed or denied based on that rule and then processing stop

Because it means that if you have a deny rule and an allow rule which match the same traffic, but if the deny rule comes first, then the allow rule might never be processed.

As a catch-all showed by the asterisk in the rule number and this is an implicit deny. If nothing else matches then traffic will be denied.

Example 1:

![[Network Access Control Lists-1.png]]
If we’re using network ACLs then we’ll need to have one associated with the web subnet and it will need rules in the inbound and outbound sections of that network ACL.

Bob request come into Web tier using HTTPS (TCP 443) with the port range is the ephemeral port range. Anything not using HTTPS will be denied.

The response from Web tier also using HTTPS (TCP 443) with the port range is the ephemeral port range. Any response not using HTTPS will be denied.

> Ephemeral port range: 1024 to 65535


Example 2:
![[Network Access Control Lists-2.png]]
You’ll need to worry about this if you use network ACLs within a VPC for traffic to a VPC or traffic from a VPC or traffic between subnets inside that VPC.

Soon the inbound rule and outbound rule will become more complex. Because we need 2 rules for Bob request to Web tier, 2 rules for Web tier to App tier, 2 rules for App tier to Web tier.

If App and Web needed software updates, then we may have 4 more rules including inbound and outbound for just update.

#### Default NACL
![[Network Access Control Lists-3.png]]

When a VPC is created, it’s created with a default network ACL and this contains inbound and outbound rule sets which have the default implicit deny, but also a catch-all allow ⇒ this means that the net effect is that *all traffic is allowed*

So the default within a VPC is that NACLs have no effect. They aren’t used

#### Custom NACL
![[Network Access Control Lists-4.png]]

Custom NACLs are created for a specific VPC and initially they’re associated with no subnets.

They only have *one rule on both the inbound and outbound* rule sets, which is the *default deny*.

The result is that if you associate this custom network ACL with any subnets *all traffic will be denied*.

## Exam Powerup
- **STATELESS** - REQUEST and RESPONSE seen as different
- Only impacts data crossing subnet boundary
- NACLs can EXPLICITLY ALLOW and **DENY**
- IPs/CIDR, Ports & protocols - no logical resources
- NACLs *cannot be assigned to AWS resources* - only subnets
- Use together with Security Groups to add explicit DENY (Bad IPs/Nets)
- Each subnet can *have one NACL* (Default or Custom)
- A NACL can be associated with *MANY Subnets*

[[Security Group]]
