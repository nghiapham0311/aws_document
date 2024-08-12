> *Amazon Route 53 health checks* **monitor** the *health* and *performance* of your *web applications, web servers, and other resources*. Each health check that you create can monitor one of the following:
> - The health of a specified resource, such as a web server
> - The status of other health checks
> - The status of an Amazon CloudWatch alarm

- Health check are **separate from**, but are **used by** records
- Health checkers located **globally**
- Health checkers check *every 30s* (every 10s costs extra)
- TCP, HTTP/HTTPS, HTTP/HTTPS with String Matching
- **Healthy** or **Unhealthy**
- Endpoint, CloudWatch Alarm, Checks of Checks (Calculated)

Health checks are *separate from*, but are *used* by records inside route53. You don’t create the checks within records. *Health checks exist separately*. You configure them separately. They evaluate some things health and they can be used by records within R53.

Health checks are performed by a fleet of health checkers, which are distributed globally. This means that if you’re checking the health of systems which are hosted on the public internet, then you need to allow these checks to occur from the health checkers

The checks can be *TCP* checks where R53 tries to establish a TCP connection with the endpoint, and this needs to be successful within 10 seconds. You can have *HTTP checks* where R53 must be able to establish a TCP connection with the endpoint within four seconds, and in addition, the endpoint must respond with a HTTP status code in the 200 range or 300 range within 2 seconds after connecting

Finally, with *HTTP and HTTPS* checks you can also perform string matching. R53 must be able to establish a TCP connection with the endpoint within 4 seconds and the endpoint must respond with the HTTP status and the 200 or 300 range within 2 seconds and R53 health checker when it receives the status code it must also receive the response body from the endpoint within the next 2 seconds. R53 searches the response body for the string that you specify. The string must appear entirely in the first **`5,120 bytes`** of the response body or the endpoint fails the health check.

This is the most accurate because not only do you check the application is responding using HTTP or HTTPS, but you can also check the content of that response versus what the application should do in normal circumstances.

Lastly, the checks themselves can be one of *three types*. You can have **endpoint checks**. And these are checks which assess the health of an actual endpoint that you specify. You can use **CloudWatch Alarm** checks which react to CloudWatch alarms, which can be configure separately and can involve some detailed NOS or in-app test if you use the CloudWatch agent.

Checks can be, what’s known as calculated checks. So **checks of all their checks**. You can create health checks which check application wide health, with lots of individual components.

![[Route53 Health Checks.png]]
If 18% or more of the heal checkers report as healthy, then the health check overall is healthy. Otherwise it’s reported as unhealthy. And in most cases, records which are unhealthy or not returned in response to queries.

[[Route53 Failover Routing]]

