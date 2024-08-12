#### Problem
Nhân viên của tổ chức animals4life cần update dữ liệu về server của tổ chức đặt ở Brisbane.
Vì khoảng cách địa lý khá xa nên việc update này trở nên chậm chạp, not reliable, hay bị lỗi.
#### Solutions
##### Single PUT Upload
Mặc định khi upload một file/folder lên trên S3 thì dữ liệu được tải lên single blob trong một single stream.
Method này có vẻ đơn giản, nhưng nếu stream failed thì hành động upload cũng bị fail. Và cách để recover lại chính là upload lại từ đầu.
Ví dụ: Upload môt file 5gb nhưng ngang 4.5gb thì bị fail, thì 4.5gb dung lượng bị phí phạm, và chắc chắn là tốn thêm một khoảng thời gian rất lâu.
Khi sử dụng Single PUT Upload thì AWS giới hạn dung lượng tối đa của file/folder được upload là 5GB.
![[S3 Performance Optimization.png]]
##### Multipart Upload
Multiplepart Upload gia tăng tốc độ upload và reliability khi upload lên S3.
Nó sẽ chia data được upload thành nhiều part độc lập với nhau.
*Min size* khi sử dụng multiple part upload là **100MB**.
Data có thể được chia ra lên tới **10000 part** và mỗi part có dung lượng tối đa từ **5MB - 5GB mỗi part**.
Part cuối cùng có thể bé hơn 5MB vì nó là những gì còn lại của data.
Mỗi part có thể fail, và ta có thể restart lại quá trình upload các failed part. Vì part là độc lập với nhau, có thể fail hoặc restart việc upload mà không ảnh hưởng tới các part còn lại.
Đây là cách nên dùng khi upload data lên S3.
![[S3 Performance Optimization-1.png]]
##### S3 Accelerated Transfer.
Giả sử công ty mở rộng thêm một chi nhánh ở London. Hiện tại công ty sẽ gồm 3 team ở Úc, Mỹ, Nam Phi và bucket chứa dữ liệu sẽ được đặt ở London.
![[S3 Performance Optimization-2.png]]
Như trong hình ảnh, đường màu đỏ là đường truyền internet từ Úc tới London. Nhưng trong thực tế thì không phải như vậy, trọng thực tế thì chính là đường màu xanh. Có nghĩa là dữ liệu ở Úc được truyền qua một quãng đường rất dài mới có thể tới được London.
Để giải quyết vấn đề này, ta sử dụng S3 Accelerated Transfer.
S3 Accelerated Transfer sử dụng network của *AWS Edge Location*. Edge Location được đặt ở nhiều nơi trên thế giới.
Mặc định thì tính năng này được disable, ta cần phải enable nó lên. Có 2 ràng buộc chính:
- *Bucket Name* không thể chứa period.
- *Bucket Nam* phải tương thích với DNS.
Sau khi bật tính năng này lên thì dữ liệu của ta thay vì đi trực tiếp tới S3 Bucket thì nó sẽ đi tới Edge Location gần nhất. Sau đó sẽ sử dụng AWS Global Network, là một mạng network do AWS kiểm soát để gửi data về S3 Bucket ở London.
![[S3 Performance Optimization-3.png]]

[[S3 Key Management Service (KMS)]]