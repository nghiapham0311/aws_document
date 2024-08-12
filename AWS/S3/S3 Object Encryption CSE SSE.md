#### Fundamental
![[S3 Object Encryption CSE SSE.png]]

Quá trình mà data được gửi từ client lên S3 Enpoint được gọi là In-Transit. Hãy tưởng tượng nó như là một cái đường hầm, không ai có thể thấy nội dung bên trong từ người gửi và người nhận.

*Client Side Encryption*: có nghĩa là chính chúng ta sẽ là người encrypt data, ta sẽ là người lựa chọn tools, keychain, keyset để encrypt data đó và gửi data đó vào tunnel. Lúc này data đã ở dạng encrypt, AWS không hiểu data này và AWS lưu trữ data này vào S3 Bucket. Làm cách này sẽ cho ta sự flexible về việc lựa chọn các key, thuật toán mã hoá nhưng đổi lại toàn bộ trách nhiệm sẽ thuộc về chúng ta, tiêu tốn tài nguyên CPU,....
*Server Side Encryption*: Ta gửi raw data vào tunnel, nếu data là text thì sẽ là text, nếu data là hình ảnh thì sẽ là hình ảnh. Khi data hit S3 endpoint thì AWS sẽ thực hiện việc mã hoá data trước khi lưu vào Object. Làm cách này thì ít flexible hơn nhưng ta delegate việc quản lý key, thuật toán mã hoá cho AWS.
#### Server Side Encryption
Bucket thì không encrypt, nhưng **Object thì được encrypt**.
SSE là **bắt buộc** với Object mà được lưu trữ trong S3.
Mỗi Object trong bucket có thể sử dụng những encrypt setting khác nhau.
Có 3 loại SSE:
###### Server Side Encryption with Customer Provided Key (SSE-C)
![[S3 Object Encryption CSE SSE-1.png]]
Người sử dụng chịu trách nhiệm quản lý Key, lựa chọn thuật toán mã hoá.
Sau đó cung cấp data dạng Plain cùng với Key, nói là dạng Plain nhưng nó đã được mã hoá bởi Https trong tunnel vì vậy người ngoài vẫn không thể thấy được data này là gì.
Sau khi hit S3 Endpoint thì data sẽ được encrypt sử dụng Key và một mã Hash được tạo ra và tag cho object mà lưu cái data này, sau đó cái Key bị huỷ đi.
Giải mã thì ngược lại, cung cấp cái Key, S3 so sánh cái Key với mã Hash để xem Hash có được generate bởi Key hay không, nếu đúng thì huỷ key, decrypt và trả lại data.
###### Server Side Encryption with S3 Managed  (SSE-S3) (Default)
![[S3 Object Encryption CSE SSE-2.png]]
Người sử dụng cung cấp data dạng Plain, và cũng như trên Https đã mã hoá để người ngoài không thể quan sát được data là cái gì.
Data sẽ được mã hoá bởi một **Unique Key**. Mỗi Object sẽ có một Unique Key để mã hoá data. S3 chịu trách nhiệm generate cái Unique Key này cho chúng ta.
S3 sẽ chịu trách nhiệm quản lý Key cho ta, ta không có quyền gì, không được thay đổi, không được lấy Key, không được làm gì hết, S3 lo hết.
S3 Key này chịu trách nhiệm mã hoá những Unique Object Key và sau đó discard đi. Để lại Cipher Text Object và Cipher Text Key.
Nếu ta cần quản lý Key như rotate, thêm sửa xoá thì sử dụng option này chưa phù hợp.
Bên cạnh đó ta sẽ có giới hạn role seperation, người nắm quyền admin của S3 có khả năng thêm, sửa, xoá đọc data mà ta không thể làm gì có thể ngăn cản họ được. Nếu công ty phân quyền gắt thì vậy là hỏng.
###### Server Side Encryption with KMS Provided Key  (SSE-KMS)
![[S3 Object Encryption CSE SSE-3.png]]
Ta vẫn gửi plain data được Https mã hoá như bình thường.
Khi data hit S3 Endpoint thì S3 sẽ request một KMS Key để mã hoá dữ liệu.
Khi đó KMS Service sẽ cung cấp Key với 2 version, 1 phiên bản là Plain Text của Key, 1 phiên bản là Key đã được mã hoá.
Sau đó S3 sẽ sử dụng Plain Key để mã hoá data rồi discard Plain Key.
Tiếp theo Ciphertext Data Encryption (DEK) cùng với data đã được mã hoá sẽ được store vào Object.
Lưu ý mỗi Object sẽ được có 1 Unique Key riêng được generate bởi KMS Service, tương tự với Option 2.
Sử dụng Option này sẽ cho chúng ta khả năng Role Seperation. Ví dụ Bob là người có full quyền quản lý ở trên S3, Bob có thể xem, xoá, thêm dữ liệu dưới dạng mã hoá, đọc cũng vô ích, bởi vì Bob không được cấp quyền sử dụng KMS Key cũng như quyền mã hoá và giải mã.
#### Summary
![[S3 Object Encryption CSE SSE-4.png]]
[[S3 Bucket Key]]