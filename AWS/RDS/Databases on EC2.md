![[Databases on EC2.png]]
Generally when people think about *running databases on EC2*, they picture one of 2 things:

- A *single instance* and *on this instance*, you’re going to be *running* a *database platform*, an application of some kind and perhaps a web server such as Apache
- A simple *split architecture* where the *db is separated* from the web server and application. So you’ll have 2 instances, probably smaller instances than the single large one.

If you have a split architecture like on the right, you can either have *both EC2 instances inside the same AZ* **or** you could *split the instances across 2*: so AZA and AZB

When you change the architecture in this way, you need to understand that you’ve introduced *a dependency into the architecture* is that there needs to be **reliable communication between the instance** ruling the application and the database instance. If not the application won’t work.

If you do decide to split these instances *across multiple AZ*, then you should be aware that there is a **cost** for *data* when it’s *transiting between different AZ* **in** *the same region.*

## Why might you do it…
- Access to the DB Instance **OS**
- **Advanced DB Option tuning** … (DBROOT)
- … Vendor demands..
- **DB or DB version AWS don’t provide**
- Specific **OS/DB Combination** AWS don’t provide
- Architecture AWS don’t provide (replication/resilience)
- Decision makers who **`just want it`**

You might need access to the OS of the db and the only way that you can have this level of access is to run on EC2 because other AWS products don’t give you OS level access.

This is one of those things though that you should really question if a client requests it because there aren’t many situation where OS level access is really a requirement. Do they need it?

There are some db tuning things which can only be done with ROOT level access.

It’s worth noting often that it’s an application vendor demanding this level of access, not the business themselves. A lot of software vendors now explicitly support AWS’s managed db products.

> **But situations certainly do exist which require you to use db on EC2**
## Why you shouldn’t really…
- **Admin overhead** - managing EC2 and DBHost
- **Backup** / DR Management
- EC2 is **single AZ**
- **Features** - some of AWS DB products are amazing
- EC2 is **ON** or **OFF** - no serverless, no easy scaling
- **Replication** - skills, setup time, monitoring & effectiveness
- **Performance** … AWS invest time into optimization & features

Don’t underestimate the effort required to keep an EC2 instance patched or keep a db host running at a certain compatible level with your application. You might not be able to upgrade or you might have to upgrade and keep the db version running in a very narrow range in order to be compatible with the application. And whenever you perform upgrades or whenever you’re fault finding you need to do it out of core usage hours which could mean additional time, stress and costs for staff to maintain both of these components

If your business has any disaster recovery planning running db on EC2 adds a lot of additional complexity

If you’re running on an EC2 instance, keep in mind you’re running on an EBS volume in an EC2 instance, both of those are within a single AZ. If that zone fails access to the db could fail and you need to worry about taking EBS snapshots or taking backups of db inside the db server and putting those on storage somewhere.

EC2 does not have any concept of serverless because explicitly it is a server. You’re not going to be able to scale down easily or keep up with bursty style demand

If you’ve got an application that doesn’t need replication, there are the skills to set this up, the setup time, the monitoring and checking for its effectiveness and all of this tends to be handled by a lot of AWS’s managed db products

[[Relational Database Service (RDS) Architecture]]