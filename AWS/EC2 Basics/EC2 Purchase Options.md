Also know as *Launch Types*
#### On Demand
![[EC2 Purchase Options.png]]

*Default Options*.
*Starting* with On Demand, if have any *specific requirement* then *move away*.

Instances of different sizes when launched using *On-Demand* will *run* on these EC2 hosts. And *different AWS customers*, they’re all mixed up on the shared pool of EC2 hosts. So even though instances are isolated and protected, different AWS customers launch instances which *share the same pool of underlying hardware*.

In term of the price, On-Demand uses *per-second billing*, and this happens while *instances are running*, so you’re paying for the resources that you consume, if you *shut an instance down*, logically, you *don’t pay* for those resources. Other *associated services* such as *storage* which do *consume resources* regardless of if the instance is *running or in a shutdown state* do **charge** constantly while those resources are being consumed.

The situation should it be used for?

With On-Demand, there are **no interruptions**. You launch an instance, you pay a per-second charge, and barring any failures, the instance runs until you decide otherwise. You **don’t receive any Capacity Reservations** with On-Demand.

If AWS has a major failure and capacity is limited, the *Reserved purchase* option receives *highest provisioning priority* on whatever capacity remains. ⇒ So if something is *critical* to your business, then you should **consider an alternative** rather than using *On-Demand*

On-Demand offers *predictable pricing*. It’s defined *upfront*, you pay a constant price, but there are no **specific discounts**

So On-Demand is suitable for **short-term workloads**. Anything which you just need to provision, perform a workload, and then terminate is ideal for On-Demand.

If you’re *unsure* about the duration or the type of workload ⇒ On-Demand is ideal

If you have *sort term of unknown workloads* which definitely *can’t tolerate any interruption*, then On-Demand is the perfect purchase option.

#### Spot
![[EC2 Purchase Options-1.png]]
*cheapest way* to get access to AWS capacity.

Spot pricing is AWS *selling that spare capacity* at a *discounted* rate. The way that it works, is that within *each region* for each type of instance, there is a given amount of *free capacity on EC2 host at any time*. AWS *tracks* this, and it *publishes a price* for how much it *costs* to use that capacity ⇒ This price is the *Spot price*

You can offer to *pay more* than the Spot price, but this is a *maximum*. You’ll only ever pay the current Spot price for the type of instance in the specific region where you provision services

Now, the *free capacity* is getting a little bit on the *low* side, AWS are getting nervous, they know that they need to free up capacity for the normal On-Demand instances which they know are about to launch. So they *up the Spot price to 4 gold coins*. ⇒ Customer one is fine because they’ve set a maximum price of 4 coins. So now they start paying 4 coins, because that’s what the current Spot price is. Customer 2nd, they’ve set their maximum price at 2 coins, so their *instances are terminated.*

>If the Spot price goes above your maximum price than any Spot instances which you have a terminated

Because *Spot instances* should **not be viewed as reliable**.

Spot pricing offers up to a **90%** reduction versus the price of *On-Demand*.

There are some significant trade-offs that you need to be aware of.

- You should *never use* the Spot purchase option for workloads *which can’t tolerate interruptions*. No matter how well you manage your maximum at Spot price, there are *going to be periods* when instances are **terminated**. ⇒ This means that workloads such as domain controllers, mail servers, traditional websites, or even flight control systems are all bad fits for Spot instances.

The types of scenarios which are *good fits for using Spot instance* are things which are **not time critical**. Since the Spot price changes throughout each day and throughout days of the week, if you’re able to process workloads around this, then you can take advantage of the maximum cost benefits for using spot

> Anything which **can tolerate interruption** and just **rerun** is ideal for Spot instances

If you have highly parallel workloads which can be broken into hundreds or thousands of pieces, maybe scientific analysis and if any parts which fail can be rerun then Spot is ideal

Anything which has a bursty capacity need, maybe media processing, image processing, any costs sensitive workloads which wouldn’t be economical to do using normal On-Demand instances, assuming they can tolerate interruption, these are ideal for Spot.

Anything which is stateless, where the state of the user session is not stored on the instance themselves, meaning they can’t handle disruption ⇒ ideal for using Spot

> Don’t use Spot for anything that’s **long-term**, anything that requires **consistent, reliable, compute**. Any business critical things, or things cannot tolerate disruption. ⇒ Don’t use Spot, it’s an anti-pattern
#### Reserved
![[EC2 Purchase Options-2.png]]
Reserved is for *long-term consistent usage of EC2*

They are a *commitment* made to AWS for the *long-term consumption of EC2 resources*

If you utilize this instance, you’ll build the normal per-second rate because you don’t have any reservation purchased which apply to this instance

The effect of a reservation would be to **reduce the per-second cost** or **remove it entirely** depending on what *type of reservation* you purchase

If you have an *unused reservation*, you still *pay* for that but the *benefit is wasted*. Reservations can be purchased for a particular type of instance and locked to an AZ specifically or to a region

If you *lock* a reservation to an *AZ*, it means that you can *only benefit* when *launching instances into that AZ*, but it also reserves capacity

If you purchase reservations for a *region* it **doesn’t reserve capacity** but it can *benefit* any instances which are *launched into any AZ in that region*.

It’s also possible that reservations can have a partial effect. ⇒ In the event that you have a reservation, you’d get a discount of a partial component of that larger instance

> *Reservations* at a high level, a way you **commit to AWS that you will use resources** for a *length of time*. In return for that commitment you get those **resources cheaper**. They key thing to understand is that once you’ve *committed*, you *pay*. Whether you *use those resources or not*

There are some choices in the way that you pay for reservations.

First, the term, you can commit to AWS either for one year or for three yeas. The discounts that you receive if you commit for 3 years are greater than those for 1 year

There are also different payment structures:

- No-Upfront: with this method, you agree to 1 or 3 year term and simply pay a **reduced per-second fee**. You pay this whether the instances running or not. It doesn’t impact cash flow
- All Upfront: whole cost of the 1 or 3 year term in advance when you purchases the reservation. If you do this, there’s **no per-second fee** for the instance and this method offers **the greatest discount**
- Partial Upfront: you pay a *smaller lump sum in advance* in exchange for a **lower per-second cost**. This is a good middle ground. You have lower per-second costs than No Upfront and less upfront costs than All Upfront. ⇒ It’s an excellent compromise if you want good cost reductions, but don’t want to commit for cashflow reasons to paying for everything in advance.

Reserved Instances are ideal for components of your infrastructure which have *known usage, require consistent access to compute and you require this on a long-term basis*. You’d use this for any components of your infrastructure that you require the *lowest cost, require consistent usage* and *can’t tolerate any interruption*.
#### Scheduled Reserved Instances
![[EC2 Purchase Options-5.png]]
*Scheduled Reserved instances* are pretty *situational* but when faced with those situations offer some great advantages

They’re great for when you have long-term requirements but that requirement isn’t something which needs to run constantly.

Let’s say you have some *batch processing* which needs to run daily for *five hours*. You know this usage is required *every day so it is long-term*. It’s known usage and so you’re comfortable with the idea of locking in this commitment but a Standard Reserved Instance won’t work because it’s not running 24/7, 365.

> A Scheduled Reserved Instances is a **commitment**. You specify the **frequency**, the **duration** and the **time**

There are some restrictions, it *doesn’t support all instance, types or regions* and you need to purchase a *minimum of 1200 hours per year*. And while you are reserving partial time blocks the commitment that you make to AWS is *at least one year*.

So the **minimum period** that you can buy for Scheduled Reserved is **one year** ⇒ this is ideal for *long term known consistent usage* but where you *don’t need* it for the *full time period*.

If you only need access to EC2 for *specific hours in a day*, *specific days of the week or blocks of time every month* then you should look at **Scheduled Reserved Instances**
#### Capacity Reservations
![[EC2 Purchase Options-6.png]]
First, AWS deliver on any commitments in terms of reserved purchases. And then once they’ve been delivered then they can satisfy any on-demand requests (priority 2nd). After both of those have been delivered then any *leftover capacity* can be used via the *spot purchase option*.

Capacity Reservation can be useful when you have a requirement for some compute which can’t tolerate interruption

If it’s a business critical process which you need to guarantee that you can launch at a time that you need that compute requirement then you definitely need to reserve the capacity

Capacity reservation is different from Reserved Instance Purchase

- Billing component
- Capacity Component

Both of these can be used in combination or individually

There are situations where you might need t*o reserve some capacity* but you **can’t justify a long-term commitment to AWS** in the form, a Reserved Instance Purchase

First, we could purchase a reservation but make it a regional one → This means that we can launch instances into either AZ in that region and they would benefit from the reservation in terms of billing.

By purchasing a regional reservation, you get billing discounts for any valid instances, launched into any AZ in that region.

So while region reservations are flexible, they *don’t reserve capacity in any specific AZ* → When you’re *launching* instances even if you have a *regional reservation* you’re launching them with the *same priority as on-demand* instances

Zonal reservations give you the same billing discounts that are delivered using regional reservations but they also reserve capacity in a specific AZ but they only apply to that one specific AZ. → If you *launch* instances into *another AZ in that region*, you get neither benefit, you pay the *full price* and don’t get *capacity reservations*

Whether you pick *regional* or *zonal* reservations you still need to **commit** for either a **one or three years term to AWS**

There are just some situations where you’re not able to do that. If the **usage isn’t known** or if you’re **not sure about how long the usage will be required** then often you **can’t commit to AWS for a one or three year** reserved term purchase but you still need to reserve the capacity.

You can choose to **use on-demand capacity reservations**.

With on-demand capacity reservation, you’re booking capacity in a *specific AZ* and you always pay for that capacity regardless of if you consume it. → You still do need to do a planning exercise and plan exactly what capacity you require because if you do book the capacity and don’t use it, you will still incur the charge.

Capacity Reservations **don’t have** the same one or three year **commitment** requirements that you need for reserved instances.

You’re not getting any billing benefit when using capacity reservations, you’re just as the name suggests reserving the capacity

You can book a capacity reservations
- If you know you **need some EC2 capacity** *without* *worrying* about the one or three year term **commitments** but you **don’t benefit from any cost reduction**
- Should look at a certain point to evaluate whether reserved instances are gonna be more economical
#### Dedicated Host
![[EC2 Purchase Options-3.png]]
Dedicated Host is an *EC2 host* which is **allocated to you in its entirely**. So you pay for the host itself, which is designed for a specific family of instances.

> You pay for the host, *any instances* which *run on that host* **have no per-second charge**. And you can launch various different sizes of instances on the host, consuming all the way up to the complete resource capacity of that host

If the Dedicated Hosts *run out of capacity*, then you *can’t* launch any additional instances onto those hosts.

> The reason that you would use Dedicated Hosts normally is that you might have software which is licensed based on sockets or cores in a physical machine

Dedicated Hosts also have a feature called Host Affinity linking instances to certain EC2 hosts. If you stop and start the instance, it remains on the same host

> In real situations and in the **`exam`** the reason to use this purchase option is for the **socket and core licensing requirements**.
#### Dedicated Instances
![[EC2 Purchase Options-4.png]]
- `Default/Shared` which On-Demand and Reserved uses. So EC2 *hosts are shared*. You will have some instances, *other customers of AWS* will have some instances, and in addition, there’s also likely to be some unused capacity. So at this model, there’s no real exposure to EC2 hosts. You’re billed per-second. That’s obviously depending on whether you use reservations but there’s no capacity to manage
- `Dedicated Hosts` : you pay for the *host*. And so *only your instances run on these hosts*. Any unused capacity is wasted and you have to manage the capacity both in terms of the under utilization, but you also need to be aware that because they’re physical host, they have a physical capacity and so there’s going to be a maximum number of instances that you can launch on these Dedicated Hosts
- `Dedicated Instances`: Your instances run on an *EC2 host* with other instances of yours and **no other customers use the same hardware**. Crucially, you don’t pay for the host nor do you share the host. You have the host all to yourself. So you will launch instances that are allocated to a host and AWS commit to not using any other instances from other customers on that same hardware.

There are some extra fees that you need to be aware of with this purchase option.
- You pay a one-off hourly fee for any regions where you’re using Dedicated Instance. This is regardless of how many you’re utilizing
- There’s a fee for the Dedicated Instances themselves.

> Dedicated Instances are common in sectors of the industry where you have really **strict requirements** which mean that you **can’t share infrastructure**

#### EC2 Savings Plan
- A **hourly commitment** for a 1 or 3 year term
- A reservation of **general compute $ amounts** ($20 per hour for 3 years)
- … Or a specific **EC2 Savings Plan** - flexibility on size & OS
- Compute products, currently **EC2, Fargate & Lambda**
- Products have an **on-demand rate** and a **savings plan** rate
- Resource usage consumes savings plan commitment at the reduced saving plan rate
- Beyond your commitment … **on-demand is used**

[[Instance Status Check & Auto Recovery]]