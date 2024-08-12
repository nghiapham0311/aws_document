*CloudFront Behaviors* control much of the TTL, protocol and privacy settings within CloudFront

You *can select the security policy to use*. So there are various different security policies and AWS update these periodically. This is generally a trade off because if you pick a more recent security policy, then potentially you can prevent any customers with older browsers accessing your distribution. So you need to pick the one that’s the best balance of security and accessibility for your users.

You’re also *able to restrict viewer access to a behavior*. So this is different than restricting access to an S3 origin. This is the option that sets the entire behavior to be restricted or private. *If you select this, you need to specify the trusted authorization type which is trusted key groups or trusted signers.*

Key groups are the new way of doing this and signers are the legacy way. If you see trusted key groups or trusted signer, then you know that it is set to restrict viewer access and you need things like signed cookies or signed URLs in order to access the content.
[[CloudFront - TTL and Invalidations]]
