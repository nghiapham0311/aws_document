Similar to KMS. Create, managers and secures cryptographic material or keys.
But still some different with CloudHSM.

Start with KMS. KMS is the key management service within AWS. So it's used essential for encryption within AWS and integrates with other AWS products.
It can generate keys, can manage keys. Other AWS services integrate with it for their encryption.
But it has one security concern. It's a shared service. While your part of KMS is isolated, undercover, you're using a service which other accounts within AWS also use.

CloudHSM is the true "Single Tenant" Hardware Security Module (**HSM**) that's hosted within the AWS cloud.
AWS provisioned but they have no access to the part of the unit where the keys are stored and managed. 
Fully **FIPS 140-2 Level 3** (*KMS* is **L2** overall, **some L3**).
Get access to CloudHSM using Standard APIs - **PKCS#11**, (**JCE**) Java Cryptography Extension, (**CNG**) Microsoft CryptoNG.
KMS can use CloudHSM as **a custom key store**, CloudHSM integration with KMS.
Enable Transparent Data Encryption (**TDE**) for **Oracle** Databases.

[[AWS Config]]