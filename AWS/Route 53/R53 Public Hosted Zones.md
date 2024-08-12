A public hosted zone is a *container* that *holds information* about *how you want to route traffic on the internet* for a *specific domain* which is accessible from the public internet
#### R53 Hosted Zones
- A **R53 Hosted Zone** is a DNS DB for a domain e.g. [animals4life.org](http://animals4life.org)
- **Globally resilient** (multiple DNS Servers)
- Created with domain registration via R53 - can be created separately
- Host **DNS Records** (e.g. A, AAAA, MX, NS, TXT …)
- Hosted Zones are what the DNS system references - **Authoritative** for a domain e.g. [Animals4life.org](http://Animals4life.org)

Hosted Zones are created automatically when you register a domain using Route53. In summary, hosted zones **are databases** which are referenced via delegation using name server records
#### Public Hosted Zones
- DNS Database (**zone file**) hosted by R53 (**Public Name Servers**)
- Accessible from the **public internet & VPCs**
- Hosted on **`4`** R53 Name Servers (**NS**) specific for the zone
- … use `**NS records**` to point at these **NS** (connect to global DNS)
- Resource Records (**RR**) created within the Hosted Zone
- Externally registered domains can point at R53 Public Zone

A public hosted zone is a DNS database, so a zone file, which is hosted by R53, on public name servers. This mean it’s *accessible* from the public *internet* and within *VPCs* using the R53 resolver.

Architecturally, when you create a public hosted zone, R53 allocates 4 public name servers, and it’s on those names servers that the zone file is hosted.

To *integrate* it with the *public DNS system*, you *change* the **name server records** for that domain to **point** at those **4 R53 name servers**

Inside a public hosted zone, you create resource records which are the actual items of data, which DNS uses.

![[R53 Public Hosted Zones.png]]

We start by creating a *public hosted zone*. Creating this allocates **4** R53 **name servers** for this zone. Those name servers *are all accessible from the public internet*. They’re also accessible from *AWS VPCs using the R53 resolver*, which assuming DNS is enabled for the VPC, is directly accessible from an internal IP address of that VPC.

Inside this hosted zone, we can create some resource records. In this case, a WW record, 2 MX records for email and a TXT record. Within the VPC, the access method is direct. The VPC resolver using the VPC `+2` address. And this is accessible from any instances inside the VPC, which use this as their DNS resolver. So they can query the hosted zone as they can any public DNS zone using the R53 resolver.

From a public DNS perspective, the architecture is the same in that the same zone file is used, but the mechanics are slightly different. DNS starts with the DNS root servers, and these are the first servers queried by our user’s resolver server.

DNS resolver server, which queries the root servers. The root servers have the information on the .org top level domain. And so the ISP resolver server can then query the .org servers. These servers hosts the .org zone file and this zone file has an entry for [animals4life.org](http://animals4life.org), which has 4 names servers and these all point at the R53 public name servers for the public hosted zone for animals4life. This process is called walking the tree and this is how any public internet host can access the records inside a public hosted zone using DNS

They’re just a zone file which hosted on 4 name servers provided by R53. This public hosted zone can be accessed from the public internet or any VPCs, which are configured to allow DNS resolution

[[R53 Private Hosted Zones]]