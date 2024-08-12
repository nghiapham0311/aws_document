![[Route53 Geolocation Routing.png]]
In many ways geolocation routing is similar to latency only instead of latency the location of customers and the location of resources are used to influence resolution decisions

With geolocation routing when you create records you tag the records with the location. Now this location is generally a country. So using ISO standard country codes it can be continents again, using ISO continent codes such as SA for South America in this case or records can be tagged with default.

There’s a fourth type which is known as a subdivision in America you can tag records with the state that the record belongs to.

When a user is making a resolution request an IP check verifies the location of the user. Depending on the DNS system this can be the user directly or the result of the server. But in most cases, these are one and the same in terms of the user’s location

So we have the location of the user and we have the location of the records.

> Geolocation doesn’t return the closest record. It only returns relevant records. When a resolution request happens route 53 takes the location of the user and it starts checking for any matching records.

First, if the user doing the resolution request is based in the US then it checks the state of the user and it tries to match any records which have a state allocated to them. If any records match they’re returned and the process stops. If no state records match then it checks the country of the user

If any records are tagged with that country then they’re returned and the process stops. Then it checks the continent. If any records match the continent that the user is based in they’re returned and the process stops.

You can also define a default record which has returned if no record is relevant for that user if nothing matches though so there are no records that match the user’s location and there’s no default record, then a no answer is returned.

> **This type of routing policy does not return the closest record. It only returns any which are applicable or the default or it returns no answer.**

So geolocation is ideal if you want to **restrict content, for example, providing content for the US market only**. If you want to do that, then you can create a US record and only people located in the US will receive that record as a response for any queries.

You can also use this policy type to **provide language specific content or to load balance across regional endpoints based on customer location**

[[Route53 Geoproximity Routing]]