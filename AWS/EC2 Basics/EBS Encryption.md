![[EBS Encryption.png]]
By *default*, *no encryption is applied*. 

An *EBS encryption* helps to mitigate this risk. EBS encryption provides at rest *encryption for volumes and for snapshots*.

When you create an encrypted EBS volume initially, EBS *uses KMS and a KMS key*, which can either be the EBS default AWS managed key.

That key is *loaded into the memory of the EC2 host* which will be using it. The key is only ever held in this decrypted form in memory on the EC2 host which is using the volume currently.

So the key *is used by the host* to *encrypt and decrypt* data between an *instance and the EBS volume*, specifically the raw storage that the EBS volume is stored on.

> This means the data stored onto the raw storage used by the volume, is ciphertext, it’s encrypted at rest.

Data only exists in an *unencrypted form* inside *the memory of the EC2 host*.

When the EC2 instance *moves from this host to another*, the decrypted key is *discarded*, leaving only the *encrypted version with the disc*.

For that instance to use the volume again, the encrypted data encryption *key needs to be decrypted and loaded into another EC2 host*.

If a snapshot is made of an encrypted volume, the same data encryption key is used for that snapshot, meaning the snapshot is also encrypted. Any volumes created from that snapshot are themselves also encrypted using the same data encryption key and so they’re also encrypted
## Exam Power UP
- Accounts can be set to **encrypt by default** - default KMS Key.
- Otherwise **choose a KMS Key** to use. KMS Key used to *generate* Data Encryption Key.
- Each volume uses **1 unique DEK**.
- **Snapshots** & **future volumes** use the same DEK.
- One encrypt *cant never change back* to un-encrypted. Work around by using OS itself clone data <= not recommended.
- OS isn't aware of the encryption ... no performance lost (because it's **between EC2 Host and EBS**, not EC2 instance).
- Using **AES-256**

[[Network Interface & Interface IPs and DNS]]