![[CloudFront - TTL and Invalidations.png]]
![[CloudFront - TTL and Invalidations-1.png]]
Our first customer makes a request for the picture and this image is not stored on the edge location, and so an origin fetch happens where the images is retrieved from the origin and placed on the edge location before being returned to the customer, which is the response.

Let’s say that the image is replaced in the origin. We take away this picture and we replace it with a much better one.

What happens when another customer makes a request to the same edge location? This time the older is returned. Because it’s the one that’s cached at the edge location. From an object perspective, it’s still viewed as valid. Even though the origin has a new version, it’s never checked because the copy that’s cached in the edge location is viewed as valid by CF.

At some point every object that’s cached by CF will expire, and when that happens, it’s not immediately discarded. but now it’s stale. It’s viewed as not current.

If at this point another customer requests a copy of this object, then the edge location doesn’t immediately return the object. Instead, as with step two, it forwards this on to the origin and the origin will respond in one of 2 ways.

Now this is based on the version of the object that’s cached at the edge location versus the one that’s stored in the origin. It’s either current or the origin has an updated object. If it’s current, then a 304 Not Modified is returned and the object is delivered directly from the edge location to the customer, and the copy of the object in the edge location once again becomes viewed as current. If there is a difference between the edge location and the origin, so if the origin has a newer version of the object, then a 200 OK message is returned along with a new version of the object which replaces the one that’s cached at the edge location. That’s how an edge location behaves within the CF network.
#### TTL (Time To Live)
![[CloudFront - TTL and Invalidations-2.png]]
An edge location views an object as not expired when it's within its Time To Live (TTL) or time to live period.

A general rule, the **more often the edge location** *delivers objects directly to your customer, which is called a cache hit, the lower the load will be on your origin and the better performance for the user*. So where possible, we want to avoid edge locations having to perform origin fetches to our origins because that will mean that CF is performing better.

Values below the Minimum TTL of the behavior will mean that the Minimum TTL is used rather than the per object setting. And likewise, for any per object TTLs specified which are above the Maximum TTL for the behavior will mean that the Maximum TTL value is used instead of the per object setting.
#### Invalidations
![[CloudFront - TTL and Invalidations-3.png]]
What a *cache invalidation* does is *immediately expire any objects **regardless** of their TTL based on the invalidation pattern that you specify*.

If you are regularly having a need to update individual files or invalidate individual files, then you might want to think about using versioned file names instead.

An example of versioned file name would be `whisker1_v1.jgp` and then update your application to point at the new version, and this requires no invalidation.

- Because you’re using a different name for the object, it means that even if those objects are cached in a customer’s browser, it won’t be used because you’re changing the name and the application points at the new name. Even data cached in a user’s browser won’t impact the image or the object that your customers see
- It means that logging is more effective because you know which actual object was used because nothing has the same name. It also means that you keep all versions of all objects and these are consistent between edge locations, so you can move between them. And of course it’s less expensive because you don’t need to use continued cache invalidations.

Don’t confuse versioned file names with S3 object versioning. That’s a different thing.

S3 object versioning allows you to have different data for an object, different versions, which use the same name. CF will always use the latest object version in a bucket by default.

Using versioned file names means having different file names for different actual versions. Different data stored in different file names. And that means that each of these file names will be cached independently on every edge location, and you can move between them in a consistent way by making changes to your application.

*In exam look up cost efficient then it maybe versioned file names is the correct answer.*

[[AWS Certificate Manager (ACM)]]