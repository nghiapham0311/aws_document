#### Securing the CF Content Delivery Path
![[CloudFront - Security - OAI & Custom Origins.png]]
You can use S3 as an origin for CloudFront, but you can do it in 2 different ways.
- If you just use S3 as an origin then it’s known as an S3 origin.
- If you utilize the static web hosting feature of S3 and use this with CloudFront, then the S3 bucket is treated the same as any non-S3 origin. This is known as a custom origin
#### Origin Access Identity (OAI)
- An **OAI** is a type of **identity**
- It can be **associated** with **CloudFront Distributions**
- CloudFront **`becomes`** that **OAI**
- That **OAI** can be used in **S3 Bucket Policies**
- **DENY** all **BUT** one or more **OAI’s**

What is an OAI? It’s a type of identity. It’s not the same as an IAM user or an IAM role, but it does share some of the characteristics of both. It can be associated with CloudFront distributions and those CloudFront distributions, when they’re accessing an S3 origin, in essence, the CF distribution becomes that origin access identity.

This means when a CF distribution is accessing an S3 origin, the identity can be used within bucket policies, so either explicit allows or denies.

Generally, the common pattern is to lock an S3 origin down to only being accessible via CF so this uses the implicit default deny to apply to everything except the origin access identity.

So the origin access identity is explicitly allowed access to the bucket and everything else is implicitly denied.

![[CloudFront - Security - OAI & Custom Origins-1.png]]
We want to allow access via CF to this S3 origin and deny any direct access.

To do that, we create an origin access identity, and we associate that OAI with the CF distribution. The effect of doing this means that the edge locations gain this identity, the OAI. Then we can create or adjust the bucket policy on the S3 bucket.

We add an explicit allow for the OAI and then in its most secure form, we remove all other access, leaving the implicit deny.

At this point, any accesses from the edge locations are actually from the OAI, the virtual identity that we’ve created and associated with the CF distribution. So access from the edge locations is allowed because the OAI is explicitly allowed via the bucket policy.

Direct access though, from our Moss user would not have the OAI associated with them. Because of this, it’s implicitly denied from accessing the bucket.

OAI can be created and used on many CF distributions and many buckets at the same time.

[[CloudFront - Lambda@edge]]