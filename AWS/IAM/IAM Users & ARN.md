### IAM Users
![[IAM Users & ARN.png]]
Nếu mình có thể gọi tên một thứ gì đó thì 99% thứ đó sẽ là 1 IAM users.
Nó có thể là người (Đức ở đội dev cần truy cập vào S3 => Đức cần một IAM user).
Cũng có thể là application (MMS cần truy cập vào AWS Activemq ở trên cloud) thì MMS cũng chính là một IAM user.
### IAM Authentication Flow
![[IAM Authentication Floew.png]]
Principal đó chính là IAM user (Nó có thể là người, là máy, hoặc là một service nào đó).
Khi Principal muốn truy cập vào AWS thì phải chứng minh rằng Principal chính là người mà nó tự nhận.
Quá trình này gọi là Authentication. Principal sẽ cung cấp cho IAM *username & password* hoặc *access key* để có thể chứng minh bản thân nó.
Sau khi chứng minh xong thì Principal trở thành một Authenticated Identity. Lúc này IAM sẽ biết được các quyền hạn của Identity này thông qua các Policy Document được attach với Identity đó.
### Amazon Resource Name (ARN)
ARN có chức năng nhận diện resources trong AWS account.
ARN cho phép chỉ định một resources cụ thể khi sử dụng tên hoặc chỉ định tất cả sử dụng dấu hoa thị.
![[ARN.png]]

### Exams
- Mỗi AWS Account chỉ có 5000 IAM Users. IAM là global service nên là mỗi account sẽ có 5000, không phải region có 5000, qua region khác lại được 5000.
- IAM users có thể là member của 10 groups.
Ảnh hưởng tới quá trình mình design system.
Nếu system của mình có nhiều hơn 5000 users hoặc một organization merge với organization khác thì lựa chọn IAM sẽ không hợp lý. 
*Solutions:* sử dụng Identity Federation & IAM Roles.

[[IAM Group]]