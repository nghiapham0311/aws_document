### Basic Notes About S3
![[S3 Security 1.png]]
### Deep Dive
S3 mặc định là private, ta không cấp quyền truy cập thì không ai ngoài ta có quyền truy cập vào S3 bucket.
Identity mà có quyền truy cập vào S3 chính là root user. Còn lại ta phải cấp quyền thì mới truy cập được.
Có nhiều cách để grant quyền access vào S3, cách đầu tiên chính là sử dụng ***S3 Bucket Policy***.
### S3 Bucket Policy
- S3 Bucket Policy là một loại Resource Policy
- Nó giống như Identity Policy, nhưng được attach vào bucket thay vì attach vào identity.
- Resource Policy cung cấp thông tin về quyền hạn cho resource. Với Identity Policy ta kiểm soát việc identity có thể truy cập vào cái gì, với Resource Policy mình kiểm soát việc ai có thể truy cập vào resource.
- Identity Policy có giới hạn về kiểm soát ai có thể truy cập, nó chỉ có thể control trong AWS account thôi, ngoài AWS account thì không được. Trái với Identity Policy, Resource Policy có thể *ALLOW/DENY* việc ai có thể truy cập trong hoặc ngoài AWS account. 
- Resource Policy có thể kiểm soát được cả ALLOW/DENY *ANONYMOUS* principals, trong khi Identity Policy chỉ có thể làm được với identity của AWS mà thôi.
![[S3 Security 1.png]]
Cách để phân biệt Resource Policy và Identity Policy chính là *principal*.
Như trong hình ảnh, principal ở đây là dấu * có nghĩa rằng tất cả mọi người được phép thực hiện hành động S3:getObject trên bucket secretcatproject.
Điều này có nghĩa là mọi identities trong cùng AWS Account, identities trong Partner Account và những người khác không cần có AWS Account có thể truy cập vào bucket này.

Bucket Policy cũng có thể rất flexible, nó có thể block truy cập tới S3 từ một địa chỉ IP nào đó như phía dưới hình minh hoạ.
Cho tất cả truy cập trừ địa chỉ IP 1.3.3.7/32.
![[S3 Security-2.png]]
Nếu identity trong cùng AWS Account truy cập vào S3 Bucket thì sẽ là merge giữa Identity Policy và Resource Policy.
Nếu identity không nằm trong AWS Account thì Resource Policy kết hợp với Identity Policy ở trong cái AWS account đang cố truy cập sẽ được apply.
Nếu là anonymous thì chỉ có duy nhất Resource Policy được apply.
### Access Control List (ACLs)
ACLs là một cách để apply security lên object và bucket.
Nó là một sub resource của object hoặc bucket.
Legacy, AWS khuyến khích dùng Resource Policy.
Inflexible, và chỉ có thể cấp quyền đơn giản.
Biết có tồn tại là được, đừng quan tâm nó làm gì, một thời điểm nào đó nó sẽ biến mất.
### Block Public Access
![[S3 Security-3.png]]


> [!CAUTION] Khi nào sử dụng Identity Policy vs Resource Policy vs ACLs
> Nó sẽ tuỳ vào situation và business để mình sử dụng. Mình muốn kiểm ta việc access từ góc nhìn của bucket hoặc mình muốn kiểm tra việc grant/deny access từ góc nhìn của identity truy cập vào bucket.
> 
> Mình muốn config một user có thể truy cập vào 10 bucket hay 100 user có thể truy cập vào một bucket.
> 
> ### Identity Policy
> Nếu ta grant/deny access cho rất nhiều resources trong AWS account thì ta cần sử dụng Identity Policy bởi vì không phải service nào cũng hỗ trợ Resource Policy. Bên cạnh đó ta cần Resource Policy cho mỗi service, mà nhiều service thì trông sẽ không hợp lý. 
> Nếu ta có nhu cầu quản lý permissions ở một nơi duy nhất thì đó chính là IAM.
> Nếu mình chỉ làm việc với permissions ở trong AWS Account, không có external access (anonymous, partner account) thì Identity Policy và IAM là phù hợp.
> 
> ### Resource Policy 
> Ta có thể sử dụng Bucket Policy, Resource Policy nếu như ta quản lý quyền access trên một resource cụ thể ví dụ như là S3.
> Nếu ta cần grant quyền access cho tất cả mọi người vào một resource, hoặc tất cả mọi người trong cùng một account thì Resource Policy là phù hợp.
> Nếu ta muốn cho phép anonymous user hoặc partner account thì Resource Policy là phù hợp.
> 
> ### ACLs
> Không nên xài, gần nghỉ hươu rồi.

[[S3 Static Hosting]]
