This is a special type of IAM Role.
### Service Link Role.
- Là IAM Role được link với một AWS Service cụ thể.
- Cung cấp một số quyền hạn được pre-define bởi AWS Service.
- Cung cấp quyền hạn mà **một AWS Service** cần để tương tác với các AWS Services khác thay mặt mình.
- Có thể được khởi tạo/xoá bởi AWS Service. Hoặc bởi chúng ta trong quá trình setup cái service đó hoặc thông qua IAM.
- Không thể xoá Service Link Role cho đến khi nó không được dùng nữa.
### Pass Role.
![[Service Linked Role Pass Role.png]]

Bob có thể truy cập vào AWS Service thông qua Service Link Role.
Hoặc Bob có thể có quyền sử dụng các cái role có sẵn đã được tạo từ trước đó bởi security team để truy cập vào AWS Service. Bob làm được điều này nếu ta bật cho anh ta quyền hạn iam:PassRole và chỉ định role cụ thể trong mục Resource.

The iam:PassRole permission in AWS Identity and Access Management (IAM) allows an IAM principal (user, group, or role) to delegate permissions to an AWS service by associating an IAM role with that service. This mechanism is crucial for enabling services to perform actions on your behalf, such as accessing other AWS resources

1. **Prerequisites for Passing a Role**:
    - The principal attempting to pass the role must have the `iam:PassRole` permission in an identity-based policy, specifying the role to be passed in the `Resource` field.
    - The role being passed must be configured in its trust policy to trust the service principal of the service it's being passed to. For example, if passing a role to an Amazon EC2 instance, the role must trust the EC2 service principal (`ec2.amazonaws.com`).
    - Both the role being passed and the principal passing the role must be in the same AWS account [1](https://aws.amazon.com/blogs/security/how-to-use-the-passrole-permission-with-iam-roles/#:~:text=iam%3APassRole%20is%20an%20AWS,function%20with%20an%20IAM%20role.).

Ví dụ:
Khi sử dụng AWS CloudFormation, CloudFormation sử dụng permission thông qua identity của ta để tương tác với AWS.
Nó có nghĩa là ta phải cần permission để tạo stack, và cả permission để tạo resources mà cái stack đó tạo ra.
Chúng ta có thể pass một cái role vào CloudFormation, cái role này có thể có những quyền hạn cao hơn mà identity của ta có thể không có để tương tác với AWS Service.
[[AWS Organization]]

