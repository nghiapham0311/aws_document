### IAM Policy Document
Khi một identity muốn truy cập vào một resources nào đó thì phải cung cấp identity cho AWS.
AWS sẽ biết một identity có những quyền hạn (Policy) nào. Một identity có thể có nhiều policy khác nhau.
Cấu tạo của một Policy Document như hình minh hoạ.
![[IAM Policy Document.png]]
Một Policy Document gồm nhiều statement khác nhau. Một Statement bao gồm:
- Sid: statement id, nên có.
- Effect: Quyền hạn có thể là cho phép hoặc từ chối.
- Resource: sử dụng AWS arn format, quy định quyền hạn được áp dụng trên resource nào của aws.
- Action: Action mình muốn thực hiện trên AWS resources. Như ví dụ ở trên hình mình muốn thực hiện tất cả các action với s3 vì sử dụng dấu hoa thị.
### Rule
![[IAM Identities Policies Rule.png]]
Như ví dụ ở trong hình, chúng ta có 2 statements đang overlap với nhau.
AWS sẽ process từ trên xuống dưới.
Ở statement đầu tiên ta cho phép full access tới s3. Ta có thể thêm sửa xoá các bucket ở trong s3.
Statement thứ hai quy định, truy cập vào CatBucket sẽ bị từ chối, ta không thể thêm sửa xoá hay làm gì với CatBucket cả.
=> Nhìn hình vẽ biểu đồ ven bên phải.

> [!NOTE] Quy tắc khi sử dụng Policy áp dụng khi làm bài thi sắp xếp theo độ ưu tiên:
> 1. Nếu có 1 cái Explicit Deny (Như đường line màu đỏ) 1 cái deny thẳng ở trong khái báo thì không có gì có thể override lại cái rule này.
> 2. Explicit Allow (Đường kẻ màu xanh) có 1 cái allow access, nếu bị overlap thì Explicit Deny luôn luôn thắng.
> 3. Default Deny: Nếu không nói gì thì mặc định là Deny access (Trừ root account, root account có quyền truy cập tất cả).

![[How It works.png]]

Lấy ví dụ về Sally có 2 policies được gán quyền cho Sally.
Sally cũng là thành viên của developer group. với Policy 3
Sally muốn truy cập vào một AWS Resource. AWS Resource này cũng có Resource Policy riêng của nó.
Cách IAM Policies hoạt động ở đây là nó sẽ thu thập tất cả các Policy hiện có (Policy 1, Policy 2, Policy 3, Resource Policy) và kiểm tra cùng một lúc.
### Type of Policies
Có 2 loại Policy:
- Inline Policy
- Managed Policy
![[Inline Policy.png]]
Lấy ví dụ về 3 người đang tham gia một dự án.
Mình muốn tạo Policy để cho phép 3 người này truy cập vào Resources của dự án.
Mình tạo Policies xong, mình đem đi assign cho từng người thì được gọi là *Inline Policy*.
Nó sẽ trở thành 3 file json riêng biệt.
Nếu chúng ta thay đổi một thứ gì đó có nghĩa là ta sẽ phải thay đổi trên cả 3 file này. Để giải quyết vấn đề này, ta sử dụng *Managed Policy*.

![[Managed Policy-2.png]]

Managed Policy nên sử dụng khi mình có một số lượng identites mà có chung quyền hạn (normal user). Có 2 loại Managed Policy là *AWS Managed Policy, Custom Managed Policy.*
Ví dụ: Chúng ta đã tạo một identity là *iamadmin* và đã attach một AWS Managed Policy là **Administrator** cho identity này.

Inline Policy nên sử dụng trong các trường hợp đặc biệt và cho một số người cụ thể. Ví dụ như a Hướng cho Đức Cao quyền hạn access cho các AWS resources trên production.

[[IAM Users & ARN]]