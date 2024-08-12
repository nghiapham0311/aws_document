#### Fundamental
- Khi bật tính năng này lên, Notification sẽ được generated khi có events occur trong S3 Bucket.
- Notification có thể chuyển tới SQS, SNS, Lambda Function.
- Có thể generated event khi Object được tạo (Put, Post, Copy, CompleteMultiPartUpload).
- Cũng có thể gửi event khi Object bị xoá luôn (Delete, DeleteMarkerCreated).
- Object Restore cũng gửi. 
- Replication.
![[S3 Event.png]]
Có thể dùng **EventBridge** để làm việc này.
[[S3 Access Log]]