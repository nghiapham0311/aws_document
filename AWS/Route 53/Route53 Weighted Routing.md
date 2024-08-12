![[Route53 Weighted Routing.png]]

**Weighted routing can be used when you’re looking for a simple form of load balancing, or when you want to test new versions of software**

Like all of the types of routing policy, it starts with a hosted zone. And in this hosted zone, these records point at IP addresses. With weighted routing, you’re able to *specify a weight for each record*, and this is called the record weight.

Let’s assume 40 for the top record, 40 for the middle and 20 for the record at the bottom. How this record weight works is that for a given name, the total wight is calculated. So 40, plus 40, plus 20, for a total of 100

Each record then gets returned based on its waiting vs the total wight.

Setting a record *weight to 0* means that **it’s never returned**. So you can do this, if temporarily you don’t want a particular record to be returned, or unless all of the records are set to 0, in which case they’re all returned.

So any of the records with the same name are returned based on its weight vs the total wight.

**An individual record is returned based on its wight versus the total weight**

Now you can combined weighted routing with health checks and if you do so, when a record is selected based on the above weight calculation, if that record is unhealthy, then the process repeats, it’s skipped over until a healthy record is selected and then that one’s returned.

Health checks don’t remove records from the calculation, and so don’t adjust the total weight. The process is followed normally, but if an unhealthy record is selected to be returned, it’s just skipped over and the process repeats until a health record is selected.

If you want to have 5% of resolution requests go to a particular server which is running a new version of Catogram, then you have that option.

So weighted routing is **really useful when you have a group of records with the same name and want to control the distribution, so the amount of time that each of them is returned in response to queries**

[[Route53 Latency-Based Routing]]