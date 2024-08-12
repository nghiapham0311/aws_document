![[CloudFront - Architecture.png]]
#### CloudFront Terms
- **Origin** - The source location of your content
- **S3 Origin** or **Custom Origin**
- **Distribution** - The `**configuration**` unit of CloudFront
- **Edge Location** - Local cache of your data
- **Regional Edge Cache** - Larger version of an edge location. Provides another layer of caching

An origin is the original location of your content. An origin can either be an S3 origin or a custom origin. It’s either S3 or anything else which runs a web server and has a publicly routeable IPv4 address.

*Distribution* is the *unit of configuration within CloudFront*. To use CF, you create a distribution, and this distribution gets deployed out to the CF network.

Edge locations are smaller than AWS regions. They’re generally one or more racks in a 3rd party data center and are usually 90% storage with the odd bit of compute services tacked on for certain AWS services.

Regional edge cache, and these are much bigger than edge locations, and there are fewer of them. They’re designed to hold more data to cache things which are accessed less frequently but where there’s still a performance benefit for caching your data closer to your customers than your origins.
#### CloudFront Architecture
![[CloudFront - Architecture-1.png]]
One of the things which is created along with the distribution is the *domain name for that distribution*. It always ends in `[cloudfront.net](<http://cloudfront.net>)` and it will be unique to every distribution which you create

You can also take your own domain name and configure the distribution to use that alternate domain name.

Once you’ve configured a distribution exactly how you like it, you can deploy that distribution to the CF network, and what this actually does is push the distribution configuration to all of the chosen edge locations, meaning that these edge locations can now be used by your customers because they have the configuration stored within the distribution

Julie attempts to access this object first so Julie makes the request for whiskers.jpg and her edge location is checked for this object. If the object was locally cached in this edge location, then the object would be returned immediately and the process would stop. Julie would get the request returned quickly. This is called a cached hit, and this is a good thing.

If the object is not stored locally in an edge location, then this is called a cached miss and this is a bad thing. If this happens, then Julie’s edge location might check its closest regional cache and this is bigger, so there’s a bigger chance that the object will be stored regionally within this cache.

So the regional cache acts as a bigger cache for multiple edge locations, and so if any local edge locations have access to this object before, then it’s likely to be stored in the regional edge cache.

If the object is not in the regional edge cache, then the process that happens next is called an origin fetch. The content is fetched from the origin, and this means that whiskers.jpq is now stored on the regional cache. The regional cache pushes the object back to the edge location which originally requested it, which also means that it’s now cached at the local edge location. Once it’s locally cached in the edge location, then it’s returned to Julie and all of this happens behind the scenes.

If Julie or anyone else near that local edge location requests the object again, then it can be delivered directly from that edge location, this is a cache hit, and this will offer improved access times, so lower latency and better performance.

Moss is accessing from a different edge location, he would make his own request and this would contact his edge location, but because his local edge location doesn’t have a cached copy of that object, this will be a cache miss and so it also checks its regional edge cache. This time though, the regional edge cache does have the object based on the previous time when Julie accessed that object and so this time, it’s immediately returned to the edge location from the regional edge cache.

> By deploying CF, you can reduce the load on origins, and get improved performance for your customers globally.

![[CloudFront - Architecture-2.png]]
There are two important architectural things that you need to know about CF.

First, it integrates with ACM or the AWS Certificate Manager, so you can use SSL certificates with CF. And also **CF is for download-style operations only**. **Any uploads go direct to the origin for processing. CF performs no write caching**

**Distribution aren’t actually where a lot of the important configuration is stored, at least not directly. That’s actually contained within behaviors, which themselves are contained within distributions**

*A behavior is a configuration within a distribution*. Think of it like a *sub configuration*. It *woks on the principle of a pattern match*.

It’s easy to assume that origins are directly linked to distributions but that’s not actually how it’s architected.

*Instead, origins are linked to behaviors, which themselves are linked to distributions. So behaviors architecturally sit in the middle between origins and distributions.*

A CF distribution always has at least one behavior but it can have many more. So all CF distributions start with one behavior, the default behavior, and this has a pattern of star, which matches everything. It’s a wildcard. But you can define other behaviors which are more specific and these take priority. For example, the default behavior is used for anything not matched by another behavior, so low-security things. But let’s say that we have private images which are located in the top bucket and this is more heavily restricted.

We could define a 2nd behavior matching a more specific pattern. Let’s say img/*, so anything accessed via the distribution, which has a path of img/ and then anything else would use this other behavior.

The path pattern as the name suggests, matches a certain path and this can allow us to have different configurations within a distribution. So we can have different configuration options for different components of CF distribution. Things like TTL, policies, origins and even the public or private nature of CF may have been described as being set on the distribution but that’s not actually entirely accurate. They’re actually set on behaviors

[[CloudFront - Behaviors]]