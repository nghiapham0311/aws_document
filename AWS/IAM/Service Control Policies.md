### Definition
Là một tính năng của AWS Organization có tác dụng hạn chế quyền hạn của AWS Account thuộc Organization.
SCP đơn giản là một json document ở bên trong quy định quyền hạn của AWS Account. 
Example: 
- SCPs giới hạn việc sử dụng region của account, không cho phép sử dụng region nào ngoài US-west.
- SCPs giới hạn việc sử dụng EC2 instance của AWS account chỉ được dùng EC2 instance nào có ram nhỏ thôi.
### SCP
SCP có thể được attach vào AWS Organization như là một cái Policies chung của Org thông qua việc gắn SCP vào *Root Container*.
SCP có thể được attach vào *một hoặc nhiều* Organization Units.
SCP cũng có thể attach vào một account cụ thể thuộc AWS Organization.
![[Service Control Policies.png]]
SCP có tính inherit. 
- Nếu SCP được gắn vào root container thì tất cả AWS account thuộc AWS Organization sẽ được hưởng Policies.
- Nếu SCP được gắn vào OUs thì những AWS account thuộc OUs hoặc account nằm trong nested OUs sẽ được hưởng Policies.
- Nếu SCP được gắn vào một AWS account cụ thể thì chỉ account này được hưởng Policies.

> [!IMPORTANT]  
> Management Account là một loại account đặc biệt.
> Nếu SCP được gắn vào Management Account thông qua root container, OUs hoặc trực tiếp thì SCP vẫn không có ảnh hưởng gì lên Management Account.

SCPs are account permissions boundaries: có nghĩa rằng SCPs kiểm soát các permissions trong account. (*Kể cả root user account trong AWS Account*).
Trong những bài học trước ta được học không có cách nào restricted root user account một cách trực tiếp cả.
Ta chỉ có thể restricted quyền hạn của AWS Account xuống. Vì root account có toàn bộ quyền hạn của AWS account, lúc này có nghĩa ta đã gián tiếp restricted quyền hạn của root user account.

> [!CAUTION]  
> SCPs không cấp bất kỳ một quyền hạn nào. Nó chỉ định nghĩa cái được làm, cái không được làm với AWS Account, nhưng nó không cho phép cấp quyền.
> 
> Cụ thể SCPs sẽ giới hạn danh sách permissions trong AWS Account. Còn việc grant permissions cho account vẫn phải được chúng ta thực hiện.

### Allow List Vs Deny List
#### Deny List
Mặc định của SCPs là deny list. Có nghĩa là tất cả các permissions đều có, không có giới hạn nào (*Full Access*). Được apply mặc định cho AWS Organization và OUs nằm trong organization.

Nếu muốn có restriction trên AWS account nào thì ta phải tự mình thêm vào.
Rule vẫn như cũ: ***DENY - ALLOW - DENY***. Nếu không có cái gì hết thì mặc định là được có quyền hạn, nếu có một cái DENY trong SCPs thì cái Deny này chiếm quyền uy tiên cao nhất. Nếu không có cả 2 thì quay về mặc định. Ở đây SCPs nó hơi ngược so với IAM. Ở IAM default là cấm, ở đây default là cho tất cả quyền.
Ví dụ: Nếu ta muốn deny S3 thì có thể thêm vào một SCPs để block S3 như sau. 
![[Service Control Policies-1.png]]
Lợi ích của việc sử dụng Deny List:
- AWS có quá nhiều service, ta chỉ cần quan tâm tới service nào mà ta cần deny thôi, còn lại cho phép hết.
#### Allow List
Để implemenet allow list gồm 2 bước
- remove đi SCPs mặc định (FullAWSAccess).
- add mới một SCPs mà chỉ cho phép những permission ta cần được phép cấp cho identities.
![[Service Control Policies-2.png]]
Cách này sẽ chỉ định services nào được phép cấp quyền cho identites trong AWS account. Nhưng nó cũng có downside tức là nó mặc định chặn hết những AWS Service mà ta có thể cần chạy. Tốn thêm công để quản lý vì AWS có rất nhiều Service, ta phải thêm manually vào SCPs.
### Summary
![[Service Control Policies-3.png]]

Ở bên phải chính là SCPs, ở đây SCPs cung cấp danh sách permissions cho 4 service (EC2, S3, SQS, Kinesis).
Ở bên trái chính là Identity Policies, cấp permissions cho identities.
Như trong hình ta có thể thấy chỉ có 3 service là được phép grant permissions cho identity (EC2, S3, SQS).
Kinesis ở ngoài cùng có nghĩa là danh sách permissions của Kinesis vẫn hiển thị tuy nhiên Identity Policies không cấp quyền này cho identity.
Ở bên trái ngoài cùng là danh sách permissions của EKS, tuy nhiên SCPs không cung cấp danh sách permissions của EKS nên danh sách quyền này không hiển thị trong màn hình cấp quyền của identity policies.
[[CloudWatch Logs]]