![[R53 Simple Routing.png]]
Simple routing *starts* with a hosted zone. Let’s assume it’s a public hosted zone. With simple routing, you can create ONE record per name.

In this example, www which is an A record type. Each record using simple routing can have multiple values, which are part of that same record.

When a client makes a request to resolve www and simple routing is used, all of the values are returned in the same query in a random order. The client chooses one of the values and then connects to that server based on the value, in this case, 1.2.3.4.

Simple routing is simple, you should *use* it when you **want to route requests towards one single service**. In this example, a web server.

The limitation of *simple routing* is that **it doesn’t support health checks**. There are no checks that the resource being pointed out by the record is actually operational.

[[Route53 Health Checks]]