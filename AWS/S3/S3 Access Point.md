Cải thiện cách quản lý S3 Buckets. Nhất là khi Buckets được sử dụng bởi nhiều users/nhiều team khác nhau.
#### Fundamental
- Đơn giản hoá cách quản lý S3 Bucket/S3 Object. Tưởng tượng S3 Bucket được sử dụng bởi nhiều users/nhiều team, số lượng Object lên tới hàng triệu với nhiều tag, prefix khác nhau.
- Thường thì ta sẽ có 1 Bucket với 1 complex Bucket Policy.
- Ta có thể tạo nhiều Access Point cho một Bucket, và mỗi Access Point có Policies khác nhau. Dễ dàng hơn cho việc quản lý.
- Hơn nữa mỗi Access Point có thể bị limit network nào có thể truy cập được.
- Mỗi Access Point sẽ có các Endpoint khác nhau, ta cung cấp Endpoint này cho những người có nhu cầu cần truy cập vào S3 Bucket.
![[S3 Access Point.png]]