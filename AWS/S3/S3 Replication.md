#### Fundamental
Bao gồm 2 loại
Cross Region Replication (CRR): data được replication từ source bucket đến destination bucket across các region của AWS.
Same Region Replication (SRR): data được replication từ source bucket đến destination bucket trong cùng region.
![[S3 Replication.png]]

#### Why
Same Region Replication:
- Log Aggregation: có nhiều S3 Bucket store log cho nhiều application khác nhau. Ta có thể dùng replicate này để tổng hợp hết tất cả log vào một S3 Bucket duy nhất.
- PROD và TEST sync. Sync data từ PROD về TEST cho anh em dễ fix bug cái nào.
- Sync data: công ty lớn thì data được backup ở cùng 1 region có nhiều account khác nhau thì sẽ an toàn hơn. Ví dụ như VNDirect bị hacker tấn công mã hoá data, nhưng data đó cũng đã được replicate ở 1 AWS Account khác trong cùng Region Việt Nam.
Cross Region Replication:
- Giảm latency khi truy cập data. Data ở Việt Nam nhưng khách ở Mỹ, replicate qua Mỹ cho khách truy cập cho vui.
#### Architect
![[S3 Replication-1.png]]
Destination Bucket có thể ở cùng AWS Account hoặc thuộc một AWS khác.
Trong cả 2 trường hợp thì Replication Configuration đều được apply cho *Source Bucket*.
Replication Configuration cung cấp thông tin về Destination Bucket để có thể replicate data tới và thông tin về IAM Role để sử dụng cho quá trình replicate.
Role này được config để S3 assume. S3 sẽ tin tưởng cái role này sử dụng trong Trust Policy của nó. Role cung cấp permissions để đọc Object trong Source Bucket và replicate data vào Destination Bucket.
Quá trình replicate sẽ được encrypted sử dụng SSL.
###### Same Account
Ở same account thì Bucket được sở hữu bởi cùng một AWS Account. Có nghĩa là Bucket này trust cái AWS Account = Trust IAM = Trust cái role.
Có nghĩa là IAM Role có quyền access vào data ở Source Bucket và Destination Bucket.
###### Different Account
Bởi vì Destination Bucket nằm ở một AWS Account khác. Có nghĩa là Account này không tin tưởng cái Role.
Ta chỉ access được Data ở Source Destination.
Phải thêm một cái Bucket Policy ở Destination Bucket cho phép IAM Role ở Source Account có thể replicate data vào trong Destination Bucket.
#### Replication Option
- All Objects or subset (sử dụng tag, prefix hoặc kết hợp cả hai để filter ra subset).
- Có thể lựa chọn Storage Class để lưu trữ dữ liệu. Default là maintain storage class từ Source Destination.
- Có thể config Ownership, mặc định owner là Source Account.
- Replication Time Control dùng để đảm bảo thời gian sync giữa Source và Destination trong vòng 15 phút.


> [!CAUTION] Đi thi
> - Mặc định thì Replication không được bật và không retroactive. Tức là những data được lưu trữ trước khi tính năng này được bật sẽ không Replicate về Destination. Cả Source và Destination cần phải bật Bucket Version lên.
> 
> - Batch Replication có thể được dùng để replicate những Object đã tồn tại trước khi bật tính năng.
> 
> - Nó là One-way chỉ có replicate từ Source về Destination. Không có chiều ngược lại từ Destination về Source.
> 
> - Có thể replicate data un-encrypted, SSE-S3, SSE-KMS (Cần phải extra config vì KMS involve), SSE-C.
> 
> - Source Bucket Owner cần có permissions với Object được lưu trữ (Đọc, Ghi, Replicate).
> 
> - Không thể replicate Bucket ở state Glacier hoặc Glacier Deep Archive vì cần thời gian rất lâu để rã đông data. Replicate vậy chỉ có đi.
> 
> - Delete thì sẽ không được replicate (Nhưng có thể làm được sử dụng DeleteMarkerReplication).

[[S3 Presigned URL]]

