### Definition
- Là một public service. Có nghĩa là endpoint của service mà application connect tới nằm trong public zone của AWS, người người nhà nhà có thể connect tới service này (AWS hoặc On premise).
- Cho phép ta lưu trữ, monitor, truy cập logging data.
- CloudWatch Log có một số built in integration như EC2, VPC Flow Logs, Lambda, CloudTrail, R53,...
- Có thể generate metric dựa trên logging data - metric filter.
### How CloudWatch Log work
![[CloudWatch Logs.png]]

CloudWatch Log là regional service, lấy ví dụ ở us-east-1.
Log sẽ đến từ các Logging sources, nó có thể là AWS service, external server, API, database.
Log sẽ gửi gửi đến CloudWatch Log thông qua Log Event.
Log Event thường sẽ bao gồm timestamp và message. Có thể có nhiều hơn.
Sau đó Log Event sẽ được lưu trữ vào Log Stream. Log Stream là các Log Events đến từ cùng một sources.
Nhiều Log Stream được gộp là thành Log Group. Ví dụ như Log Group của var log của 3 instance EC2. Màu xanh là instance 1, vàng là instance 2, đỏ là instance 3.
Log Group cũng là nơi ta define ra Permissions và Settings. Setting ở trên Log Group cũng sẽ ảnh hưởng tới các Log Stream ở trong group đó.
Cũng ở tại Log Group mà metric filter được define. Nếu như có sự cố nào xảy ra thì metric filter sẽ increase metric, metric được kết nối với alarm, và alarm sẽ báo cho người sử dụng.
[[CloudTrail]]