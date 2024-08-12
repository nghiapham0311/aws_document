Virtualization is the process of running more than one operating system on a piece of physical hardware or server.
#### Before Virtualization
![[Virtualization 101.png]]
Bob & Julie tương tác với hardware thông qua application thì sẽ không được.
Chỉ có Kernel running Privileged Mode mới có quyền tương tác với hardware ở bên dưới.
#### Emulated Virtualization
![[Virtualization 101-1.png]]
With this method a host OS still run on the hardware and it included additional capability known as a *hypervisor*.

Around the multiple other OS were wrapped a container of sorts called a *virtual machine*.

Each virtual machine *was an own modified OS* with a virtualization *allocation of resources, such as CPU memory and local disc space*

The guest OSs believed these to be real. They had drivers installed just like physical devices but they weren’t real hardware

They were all emulated fake information provided by the hypervisor to make the guest OS believe that they were real

> The guest OS still believed that they were running on real hardware

So the hypervisor, it performs a process known as Binary Translation. ⇒ it means that the guest OS need no modification, but it’s really, really slow.
#### Para-Virtualization
![[Virtualization 101-2.png]]
With Para-Virtualization, the guest OSs are *still running in the same virtual machine containers with virtual resources allocated to them*.

But instead of the slow binary translation which is done by the hypervisor, Para-Virtualization *only works on a small subnet of OS*

OS which can be modified. They’re modified to make them user calls but instead of directly calling on the hardware, that calls to the hypervisor called hyper calls

By modifying the OS in this way and using power virtual drivers in the OS for network cards and storage, it means that the *OS became almost virtualization aware* and this massively *improves performance* but it was still a set of software processes designed to trick the OS and other hardware into believing that nothing has changed

#### Hardware Assisted Virtualization
![[Virtualization 101-3.png]]
With Hardware Assisted Virtualization, the hardware *itself has become virtualization aware*. The CPU contains specific instructions and capabilities so that the hypervisor can directly control and configure this support

But these *instructions can’t be executed* as is because *the guest OS still thinks that it’s running directly on the hardware*. and so they’re *redirect to the hypervisor* by the hardware.

They hypervisor handles how these are executed and this mean very little performance degradation over running the operating system directly on the hardware.

The problem though is while this method does help a lot what actually matters about a virtual machine tends to be the input output operations.

Unless you have physical network hardware per virtual machine, there’s always going to be some level of software getting in the way.

And when you’re performing highly transactional activities such as network IO or disc IO this really impacts performance, and it consumes a lot of CPU cycles on the host
#### SR-IOV
![[Virtualization 101-4.png]]
Single route IO virtualization.

It allows a *network card or any other add-on card to present itself not as one single card, but almost as several mini cards*.

Because this is supported in hardware, these are fully unique cards as far as the hardware is concerned

This means *no translation has to happen by the hypervisor*. The **guest OS can directly use its card whenever it wants**

In EC2 this feature is called **Enhanced Networking** and it means that the network performance is massively improved.

[[EC2 Architecture]]

