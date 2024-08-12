#### CNAME - The problem
- A maps a **NAME** to an **IP Address**
- … [catagram.io](http://catagram.io) ⇒ 1.3.3.7
- **CNAME** maps a **NAME** to another **NAME**
- … [www.catagram.io](http://www.catagram.io) ⇒ [catagram.io](http://catagram.io)
- CNAME is **invalid for naked/apex** ([catagram.io](http://catagram.io) địa chỉ mà không có www quần què gì hết gọi là naked/apex)
- Many AWS services use a DNS Name (ELBs)
- With just **CNAME - [catagram.io](http://catagram.io) ⇒ ELB would be invalid**

It just isn’t supported within the DNS standard. This is a problem because many AWS services such as elastic load balancers they don’t give you an IP address to use which they give you a DNS name.

This means that if you only use CNAMEs names pointing the naked [catagram.io](http://catagram.io) at an elastic load balancer, wouldn’t be supported. You could point [www.catagram.io](http://www.catagram.io) at an ELB because using a CNAME name for a normal DNS record is fine but you can’t use a CNAME for the domain apex a.k.a the naked domain. ⇒ this is a problem which ALIAS records fix.
#### ALIAS - The problem
![[R53 CNAME vs ALIAS.png]]
An ALIAS record generally maps a name onto an AWS resource.

ALIAS records can be used for **both** the *naked domain* known as the domain apex or for *normal records*.

For normal records, such as [www.catagram.io](http://www.catagram.io), you could use CNAMEs or ALIAS records in most cases. **But for naked domains, you have to use ALIAS records if you want to point at AWS resources**

ALIAS is actually a subtype. You can have an A record ALIAS and a CNAME name record ALIAS. This is confusing at first. But the way I think about this is both of them are ALIAS records but, you need to match the record type with the type of the record you’re pointing at.

With an ELB, you’re given an A record. It’s a name which points at an IP address. So you have to create an A record ALIAS if you want to point at the DNS name provided by ELB. If the record that the resource provides is an A record, then you need to use an A record ALIAS.

So you’re going to use *ALIAS* records when you’re **pointing at AWS services, such as the API Gateway, CloudFront, Elastic Beanstalk, Elastic Load Balancers, Global Accelerator and even S3 buckets**

[[R53 Simple Routing]]