### Definition
Một cái role thích hợp được sử dụng bởi một số lượng không xác định hoặc là nhiều principals khác nhau.
Nó có thể là nhiều IAM user trong cùng 1 account hoặc là nhiều service, application inside hoặc outside aws account => role này.
*Nếu mà mình không xác định được số lượng principal có thể sử dụng 1 identity thì nó có thể là roles đấy*
Hoặc là nếu mình có 5000 principals (đạt số lượng limit của iam) thì ta cũng có thể sử dụng IAM roles.
Roles cũng có thể sử dụng cho trường hợp một thứ gì (mobile application, web service) đó trở thành role trong một khoảng thời gian rồi dừng lại.
Role không phải đại diện cho bản thân bạn, role đại diện cho quyền truy cập trong AWS account. Nó là một thứ có thể được các identites khác sử dụng. Identites assume là cái role đó trong một khoảng thời gian, sử dụng những quyền hạn của cái role đó và sau đó dừng lại, không đóng vai role đó nữa.

Với role đơn giản là mình mượn quyền hạn của cái role đó trong một khoảng thời gian. Không như IAM nó là long term.

![[IAM Roles.png]]

Lấy ví dụ, mình là một mobile application, và mình nhận một role trong AWS account thì mình nhận cái role đó và có quyền hạn của cái role đó trong một khoảng thời gian nhất định. Hay nói cách khác mobile application đã trở thành một identity trong AWS account trong một khoảng thời gian ngắn.

Có gì khác nhau giữa đăng nhập với username bình thường và nhận một cái roels ? Ở trong cả hai trường hợp ta đều có những quyền hạn mà identity có.
#### Policies
IAM role có thể gắn 2 policy:
- Trust Policy: kiểm soát việc identites nào có thể become thành cái role này. Nó có thể refer tới những identities ở trong cùng AWS account (IAM user, IAM Role, S3, EC2) và những external identities (identities ở AWS account khác, Facebook, Twitter,...). Khi mình đã nhận role thì AWS sẽ generate ra một cái temp credentials, nó giống như access key nhưng nó có thời hạn sử dụng nhất định, khi hết hạn thì identity nhận role cần phải renew lại bằng cách nhận lại role. Việc generate credential này được thực hiện bởi STS (Security Token Service).
- Permission Policy: kiểm soát quyền truy cập vào các AWS resources trong AWS account.
![[IAM Roles-1.png]]
Role cũng giống như identities, nó có thể được refer từ Resources Policy. Nếu resources policy hoặc permission policy cho phép role được quyền truy cập vào S3 -> bất cứ identities nào assume cái role này cũng sẽ có quyền truy cập vào S3.
[[When Use IAM Roles]]