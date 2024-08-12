### Overview
IAM group là một nhóm các IAM users.
IAM được tạo ra nhằm mục đích quản lý một lượng lớn IAM users một cách dễ dàng hơn.

Lợi ích của IAM group:
- Có thể tổ chức IAM users vào các group nhằm quản lý một cách hiệu quả hơn.
- IAM group có thể đính kèm IAM Policy cho phép quản lý việc phân quyền hiệu quả. (Inline & Managed Policy).

![[IAM Group.png]]
Sally vừa là thành viên của Developer group, vừa là thành viên của QA group. Do đó Sally sẽ có quyền kế thừa những Policy thuộc cả 2 group này.
Bên cạnh đó Sally còn có Inline Policy được cung cấp riêng cho bạn đó.
Do đó AWS sẽ merge cả 3 Policies này lại và đem đi evaluate theo rule mình đã học ở bài học trước đó (Deny - Allow - Deny).
### Exams
- ***Không thể Login vào IAM group***, IAM group không có credential như IAM user thông thường.
- IAM group không thể có các group con nằm trong nó. Ví dụ như nhóm developer thì không thể có nhóm BE, nhóm FE nằm trong developer được. Phải tự tạo nhóm riêng. 
- IAM user có thể là thành viên của nhiều IAM group.
- Không có giới hạn về IAM users nằm trong một group. Một group có thể có số lượng maximum là 5000 users tương ứng với số lượng IAM users mà 1 account có thể có được.
- Không có một group chung bao gồm tất cả IAM users được tạo sẵn. Nếu mình muốn có loại group này thì phải tự tạo.
- Một account có thể có 300 IAM groups nhưng có thể tăng lên khi mình viết ticket nhờ AWS hỗ trợ.

![[IAM Group-1.png]]
Resource Policy là policy mình attach cho AWS Resource.
Ví dụ S3, S3 có thể attach một Policy cho phép chỉ Đức Cao có thể truy cập vào một Bucket cụ thể. Resource Policy này chỉ định người có quyền truy cập/role có quyền truy cập thông qua ARN.
Resource Policy ***không thể cấp quyền truy cập*** cho group được.

[[IAM Roles]]