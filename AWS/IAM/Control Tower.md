### Definition
- Cho phép setup nhanh và dễ dàng môi trường multi account.
- Control Tower quản lý các service khác nhau để setup multi account environment. Các service này bao gồm AWS Organization, CloudFront,.... Hãy nghĩ tới Control Tower như là một phiên bản tiến hoá của AWS Organization, thêm mới nhiều tính năng, tự động hoá nhiều hơn.
### Deep Dive
#### Term
*Landing Zone*: đây chính là multi account environment, là một phần của Control Tower. Là nơi mà phần lớn người sử dụng sẽ tương tác khi sử dụng Control Tower. (AWS Organization phiên bản cường hoá). Nó cung cấp thêm các chức năng SSO/ID Federation, Centralized Logging & Auditing.
*Guard Rail*: Detect/Mandate rules/standard across all accounts.
*Account Factory*: Chuẩn hoá quy trình và tự động hoá việc tạo mới AWS Account.
*Dashboard*: một page để quản lý toàn bộ.
#### How It Work
![[Control Tower.png]]
Control Tower cũng như AWS Organization, cần phải được tạo ra từ AWS account. Tương tự AWS account này cũng sẽ trở thành Management Account của Landing Zone.
Ở trong account này trước hết ta sẽ có Control Tower, nơi quản lý mọi thứ.
Ta có AWS Organization nơi cung cấp cho ta multiple account structure (SCPs, OUs,....).
Ta có SSO (Single Sign On) cung cấp bởi IAM Identity Center cho phép Internal ID hoặc Federated ID truy cập vào những thứ trong Landing Zone mà Identity có permissions. Tất cả đều được quản lý tới Control Tower.
Account Factory: giống như một team các con bot có tác dụng create/modifying/deleting AWS Account tuỳ theo nhu cầu của business
Khi Control Tower vừa được tạo ra nó sẽ tạo ra 2 OUs trong AWS Organization:
- Foundational OU còn được gọi là Security. Trong Foundational OU, Control Tower tạo ra 2 account Audit Account và Log Archive Account. Log Archive Account dành cho user cần truy cập vào các thông tin logging cho tất cả account trong Landing Zone. Audit Account dành cho user cần truy cập các thông tin về Audit.
- Custom OU còn được gọi là Sandbox. Control Tower sử dụng Account Factory để tạo một số lượng AWS account tuỳ thích. File config được dùng bởi Account Factory để template các Account này. Under the hood, Control Tower sử dụng CloudFormation combine với AWS Config, SCP để tạo ra các Guard rails.
#### Landing Zone
Đây là nơi mà mọi người có thể implement well architect, multi account environment. 
Home region là nơi mà mình deploy product của mình lên lần đầu (ví dụ US East 1). Mình có thể disable đi các region khác nhưng home region thì luôn luôn available.
Landing Zone được build bằng AWS Organizaion, AWS CloudFormation, AWS Config,...
Landing Zone sử dụng IAM Identity Center để cung cấp SSO, multiple accounts, ID Federation.
Landing Zone cung cấp khả năng monitoring & notification với CloudWatch và SNS.
End user có thể giám sát việc hoạt động trong Landing Zone sử dụng Service Catalog.
#### Guard Rail
Guard Rail là các rules, một cách để quản lý multiple account.
Gồm có 3 loại *Mandatory, Recommended, Elective*.
Mandatory là bắt buộc phải theo
Recommended gợi ý phải theo, không bắt buộc
Elective dùng để implement một số nhu cầu mà niche.
Guard Rail hoạt động theo 2 cách:
- *Preventive*: Stop doing thing trong AWS Account trong Landing Zone sử dụng AWS Organization SCP. (enforced, not enabled).
- *Detective*: kiểm tra định kỳ, sử dụng AWS Config để kiểm tra xem configuration của một thứ gì đó trong AWS account có match với cái mình define hay không. (clear, in violation, not enabled).
#### Account Factory
Automate việc create/modifying/delete aws account.
Có thể được sử dụng bởi cloud admin hoặc là end user (cần cung cấp permissions).
Có thể tự động apply guardrail trong việc khởi tạo account.