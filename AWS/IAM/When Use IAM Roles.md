### AWS Services.
AWS Services thay mặt ta xử lý một tác vụ nào đó và nó cần sử dụng quyền hạn để truy cập vào các AWS Resources khác nhau.
Ví dụ: Lambda Function, khi Lambda Function chạy lên nó có thể bật/tắt máy ảo EC2, thực hiện backup S3. Nó cần ta cấp quyền để làm những việc này. Thay vì phải hard code cái access key của chúng ta vào Lambda Function thì ta có thể tạo 1 IAM Role và gán cho cái Lambda này.
Cái IAM Role này có một Trust Policy cho phép khi Lambda Function execute có thể assume nó là cái role này.
Có một Permission Policy cho phép truy cập vào các AWS Resources.
Khi Function chạy nó sẽ request tới STS để lấy credentials. Dựa trên credentials này nó có thể truy cập vào AWS Resources.

![[When Use IAM Roles.png]]
Vì sao: ta có thể có một số lượng không xác định Lambda Function có thể chạy. Và khi chạy xong thì mình phải discard đi tất cả các quyền hạn của Lambda Function.  => Role cần phải sử dụng ở đây. Không nên gán cứng access token vào Lambda Function vì vấn đề security hoặc chúng ta phải rotate cái access key này.
### Production Issues.
Wayne làm việc ở team help desk. 
Ta sẽ tạo một IAM Group HelpDesk và gán quyền hạn chỉ có thể read-only.
Nhưng có trường hợp khẩn cấp xảy ra trên môi trường production, dev thì đi nghỉ hết rồi. 
Lúc đó Wayne có thể nhận role Emergency Role để start/stop một EC2 instance hộ cho khách hàng.
Và quyền hạn này chỉ diễn ra trong một thời gian ngắn nhất định cho đến khi tech team quay trở lại làm việc. Lúc đó quyền hạn của Wayne quay trở lại bình thường là read only và việc Wayne assume cái role này cũng sẽ được ghi log lại vào hệ thống.
![[When Use IAM Roles-1.png]]
### Adding AWS Account to Existing Corp Environment.
Lấy ví dụ về nashtech hiện tại đang có hơn 5000 nhân viên.
Nashtech đang sử dụng Azure Active Director để làm Identity Provider.
5000+ nhân viên naỳ cần quyền truy cập vào S3 để lấy record về townhall meeting.
Lúc này ta có thể sử dụng IAM Roles để cấp quyền truy cập cho các nhân viên của nashtech.
Why ?
- Giới hạn cứng của IAM là 5000 users, giả sử như ta có thời gian rãnh để tạo ra 5000 user và cấp quyền cho 5000 user này. Ta có nên làm như vậy không ? Rất khó để quản lý.

> [!WARNING]  
> Sử dụng role và cho Identity Provider assume cái role này được gọi là ***Identity Federation***. Nó có nghĩa là ta có một số lượng role rất nhỏ để quản lý và external users có thể assume cái role này để truy cập vào AWS Resource cần thiết.

![[When Use IAM Roles-2.png]]
### Design Application
Ví dụ ta có một application mobile có tầm 1 triệu user.
Database sẽ được chạy trên AWS sử dụng DynamoDB.
Trên các ứng dụng mobile thường sẽ có chức năng login thông qua các mạng xã hội như Facebook, Twitter,...
Ta có thể sử dụng Role ở đây, trust policy chính là credential lúc login của các mạng xã hội này. Tạo thuận lợi cho end user.
Dễ dàng quản lý role, quản lý quyền hạn của role.
Có thể scale up user dễ dàng mà không gặp giới hạn 5000 users của IAM.
Không có AWS credential nào được lưu trữ ở phía mobile application. Nên trường hợp bị hack cũng không lộ credential.

![[When Use IAM Roles-3.png]]
### Multiple Account.
Ta được partner thuê để xử lý dữ liệu lớn giúp khách hàng, và sau khi xử lý xong thì upload kết quả vào S3 của partner.
AWS account của ta có 1000+ user nhưng team IT của khách hàng không muốn tạo 1000 identity trong AWS account của họ.
Vì vậy họ tạo ra một Role cho phép upload data vào S3 và họ gán role này cho identity ở account của chúng ta.
![[When Use IAM Roles-4.png]]

[[Service Linked Role Pass Role]]