- R53 normally has 2 jobs - **Domain registrar** and **Domain Hosting**
- R53 can do **BOTH**, or either **Domain Registrar** or **Domain Hosting**
- R53 Accepts your money (**domain registration fee**)
- R53 allocates 4 Name Servers (NS) (**domain hosting**)
- R53 Creates a zone file (**domain hosting**) on the above NS
- R53 communicates with the registry of the TLD (**Domain Registrar**)
- … sets the NS records for the domain to point at the 4 NS above

When you *register* a domain using R53, it actually does *2 jobs* at the same time. while these 2 jobs are done together, conceptually, they’re two different things.

R53 acts as a domain registrar and it provides domain hosting.

> The registrar, which is the company who registers the domain on your behalf and the domain hosting, which is how you add a manage records within hosted zones.
#### R53 - Both Roles
![[Route53 Interoperability.png]]
Step *one* is to *register a domain within Route 53*. So you liaise with Route53 and you pay the fee required to register a domain, which is a per year or per three year fee.

First, the R53 registrar liaises with the R53 DNS hosting entity and it creates a public hosted zone, which allocates for R53 name servers to host that zone, which are then returned to the registrar.

Once the registrar has these 4 name servers, it passes all of this along through to the .org top level domain registry.

The registry is the manager of the .org top level domain zone file. And it’s via this entity that records are created in the top level domains zone for the [animalsforlife.org](http://animalsforlife.org) domain.

So entries are added for our domain, which point at the 4 name servers, which are created and managed by R53. At this point, we’ve paid *once* for the *domain registration* to registrar, which is R53 and we *also have to pay a monthly fee* to *host* the domain.
#### R53 - Registrar Only
![[Route53 Interoperability-1.png]]
We sill *pay R53 for the domain*. They still liaise on our behalf with the registry for the top level domain. But this time a *different entity is hosting the domain*.

This architecture involves more manual steps because the registrar and the DNS hosting entity are separate. So as the DNS admin, you would need to create a hosted zone.

The 3rd provider would generally charge a monthly fee to host that zone on name servers that they manage. You would need to *get the details of those servers* once it’s been created **and pass those details on to R53** and R53 would then liaise with the .org top level domain registry and set those names server records within the domain to point the name servers managed in this case by Hover.
#### R53 - Hosting Only
![[Route53 Interoperability-2.png]]
With this architecture, the *domain is registered via a 3rd party* domain registrar in this case, Hover.

So it’s the registrar who liaise with the top level domain registry, but we use R53 to host the domain.

At some point either when the domain is being created or afterwards, we have to create a public zone within R53. This creates the zone and the name servers to host the zone, obviously for a monthly fee.

So once this has been created, we pass those details through to the registrar who will liaise with the registry for the top level domain and then those names server records are added to the top level domain, meaning the hosted zone is now active on the public internet

Now it’s possible to do this when registering the domain. So you could register the domain with Hover and immediately provide R53 name servers, or you might have a domain that’s been registered years ago, and you now want to use R53 for hosting and record management.

You can use this architecture either while registering a domain or after the fact by creating the public hosted zone and then updating the name sever records in the domain via the 3rd party registrar and then the .org registry.