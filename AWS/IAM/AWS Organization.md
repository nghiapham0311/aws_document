### Why Using AWS Organization
Sự khó khăn trong việc quản lý nhiều AWS account.
Với mỗi AWS Account sẽ có 1 pool chứa các IAM user, cũng như các thông tin về payment khác nhau.
Đây là lý do ta sử dụng AWS Organization để quản lý thông tin của nhiều AWS Account khác nhau.
### How
Lấy một AWS account làm chuẩn. Ta sẽ sử dụng account này để tạo AWS Organization.
Lưu ý rằng AWS Organization không tự động tạo ở trong account mà chính ta phải là người sử dụng account để tạo AWS Organization.
Account này sau khi tạo AWS Organization sẽ trở thành *Management Account*.
Sử dụng *Management Account* ta có thể invite các AWS Account khác vào Organization của chúng ta. Cần phải approve như github vậy. Sau khi đồng ý thì trở thành một phần cùa Organization (*Member Account*).
Một AWS Organization chỉ có thể có **duy nhất môt** Management Account và có thể có **Không hoặc nhiều** Member Account.
![[AWS Organization.png]]
### AWS Organization Hiarachy
![[AWS Organization-1.png]]
*Root Organization*: Là container chứa AWS Accounts hoặc các *Organization Units*. Organization Units cũng là một container chứa các AWS Account hoặc các OUs khác. Root Organization ở trên đỉnh của chóp.
### AWS Organization Features.
##### Consolidated Billing
Một trong những tính năng ta cần quan tâm là *Consolidated Billing*.
Như trong hình ảnh có 4 AWS Account, mỗi account sẽ có thông tin về thanh toán khác nhau. Vì nó đã được thêm vào AWS Organization, những thông tin thanh toán của mỗi account sẽ được xoá đi. Thay vào đó, những *Member Account* sẽ để cho *Management Account* chịu tránh nhiệm trả bill.
Chỉ nhận 1 bill duy nhất cho toàn bộ account trong AWS Organization.

![[AWS Organization-2.png]]

> [!Tip]  
> Lưu ý từ giờ trở đi nếu ta thấy *Master Account, Management Account, Payer Account* thì nó đang ám chỉ tới cùng một thứ chính là *Management Account*.
##### Create AWS account inside AWS Organization
Ta có thể tạo AWS Account trực tiếp trong AWS Organization.
Ta chỉ phải cung cấp cho nó 1 unique email address.
Khi tạo AWS account theo kiểu này thì ta sẽ không cần mời AWS account join AWS Organization nữa bởi vì ta tạo trực tiếp ở trong đây rồi.
![[AWS Organization-3.png]]
Khi sử dụng AWS Account thì việc tạo IAM user cho mỗi account trong organization là không cần thiết nữa.
Thay vào đó IAM Roles có thể được sử dụng để cho phép IAM users truy cập vào các AWS account khác nhau trong cùng organization.
> [!Tip]  
> Chỉ sử dụng duy nhất 1 account AWS Organization cho việc chứa các identities có thể login. Nó có thể là *Managemet Account hoặc một account khác trong cùng Organization*.
> 
> Nếu công ty của ta đã có sẵn identity provider rồi và mong muốn sử dụng users có sẵn để truy cập vào AWS thì ta có thể sử dụng các account được chọn để config *Identity Federation*.
> 
> Sau khi đã có quyền truy cập vào Organization, ta sẽ sử dụng tính năng *role switch* để assume các cái role có trong những AWS Account khác trong cùng Organization.

![[AWS Organization-4.png]]
 [[Service Control Policies]]