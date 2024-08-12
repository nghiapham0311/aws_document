- **Ram** CPU, memory, Local Storage Capacity & Type.
- **Resource Ratios** nếu application cần xử lý nhiều CPU hơn thì chọn CPU, nếu app cần nhiều Ram hơn CPU thì chọn option nhiều ram.
- **Storage** and **Data** network **Bandwidth**.
- System Architecture/ Vendor.(ARM architecture or an x86 architecture or Intel based CPUs or AMD CPUs)
- Additional Features and Capabilities
#### EC2 Categories
- **General Purpose** - Default - Diverse workloads, equal resource ratio.
- **Compute Optimized** - Media processing, HPC, Scientific Modeling, gaming, machine learning.
- **Memory Optimized** - Processing large in memory datasets, some database workloads.
- **Accelerated Computing**- Hardware GPUC, field programmable gate arrays (FPGAs).
- **Storage Optimized*** - Sequential and Random IO - scale-out transactional databases, data warehousing, ElasticSearch, analytic workloads.
#### Decoding EC2 Types
![[EC2 Instance Types.png]]
The whole thing end to end, R5dn.8xlarge ⇒ this is known as the instance type

The *letter at the start* is the instance *family*. There are lots of examples of this, the T family, the M family and the R family

There’s lots more but each of these are designed for a specific type or types of computing

The next part is the *generation*. So the number five in this case is the generation.

- If you see instance type starting with R5 or C4. For example, C4 is a fourth generation of the C family of instance

Across to the other side, we’ve got the size. So in this case, 8xlarge or eight extra large, this is the *instance size*.

Within a family and a generation, there are always multiple sizes of that family and generation, which determine how much memory and how much CPU the instance is allocated with

There’s a logical and often linear relationship between the sizes. Depending on the family and generation, the starting point can be anywhere as all as the nano.

Next to the nano, there’s micro, then small, then medium, large, extra large, two extra large, four extra large, eight extra large and so on.

The bit which is in the middle, this can vary. There might be no letters between the generation and size but there’s often a collection of letters which denote additional capabilities.

Common examples include a lowercase `a` which signifies AMD CPU. So lowercase `d` which signifies NVMe storage, a lowercase `n`, which signifies network optimized, a lowercase `e` for extra capacity which could be RAM or storage.

![[EC2 Instance Types-1.png]]

[[Storage Refresher]]
