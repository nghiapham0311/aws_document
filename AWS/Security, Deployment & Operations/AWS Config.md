AWS Config has two main jobs.
- First is to **record changes** *over time on resources within an AWS account*. Once *enable*, every time a **resources configuration changes** **a configuration item is created which stores the configuration of that changed resource at specific point of time**.
- Great for **auditing** changes, **compliance** with standards.
- **Does not prevent changes happening** ... no protection.
- *Regional* Service ... supports cross-region and cross account.
- Changes can generate *SNS* notifications and near- real time events via EventBridge & Lambda.
- Store in **S3** Bucket.

![[AWS Config.png]]

[[Amazon Macie]]

