#### Problem with S3-KMS
![[S3 Bucket Key.png]]
Hiện tại ta đang sử dụng S3 cùng với KMS để mã hoá dữ liệu được upload lên S3.
Bob upload một image con mèo lúc này S3 sẽ request qua KMS để generate ra Data Encryption Key. Sau đó S3 mã hoá bức ảnh này rồi lưu bức ảnh cùng với cái Key được generate ra. Nên nhớ cái Key này là unique cho mỗi Object.
Tương tự với image con chó, S3 cũng sẽ request sang KMS để yêu cầu generate ra Data Encryption Key.
Tương tự với con gà.
Vấn đề ở đây chính là ở KMS, call tới KMS để generate key thì sẽ tốn phí. Hơn nữa phải đối mặt với vấn đề về throttling, chỉ được call 5500/10000/50000 request/second tuỳ thuộc vào region khác nhau.
Giới hạn về call per second ảnh hưởng tới việc scaling khi ta muốn upload nhiều dữ liệu lên S3.
#### Bucket Keys
![[S3 Bucket Key-1.png]]
Giải pháp ở đây chính là sử dụng Bucket Key.
Bob upload hình ảnh lên S3. Lúc này S3 sẽ call tới KMS đề nghị generate ra một Data Encryption Key với time limit. Sau đó lưu trữ cái Key này và sử dụng nó để generated unique encrypt key để lưu trữ và mã hoá dữ liệu.
Lưu ý Bucket Key cần phải được bật lên trong Bucket setting.
Sử dụng Bucket Key thì những Object được lưu trữ trước khi enable tính năng này sẽ không ảnh hưởng, chỉ ảnh hưởng những Object upload mới sau khi bật tính năng này.
Sau khi hết time limit, thì ta lại gọi sang KMS để generate ra cái Key mới và sử dụng Key mới để làm tiếp.
Điều này giảm số lượng API call qua KMS, dẫn tới giảm cost. Key được lưu trữ dẫn tới việc dễ dàng tăng performance, scaling.

> [!CAUTION]
> Sau khi bật Bucket Key lên thì ta sẽ nhận ít event log trên CloudTrail hơn cho KMS và nhận nhiều log event hơn cho S3 Bucket. Vì ta đã giảm số lượng call tới KMS rồi, điều này là hợp lý. Ta sẽ thấy Bucket ở trong log, không phải Object.
> 
> Làm việc tốt với replication. Bucket Key sử dụng trong replication cũng giống với Bucket Key sử dụng trong origin.
> 
> Nếu replication một plain text với Bucket Key thì plain text này sẽ được encrypt ở destination và gây ra thay đổi của ETAG.

[[S3 Object Storage Class]]