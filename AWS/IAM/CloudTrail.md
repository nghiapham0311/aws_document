### Definition
CloudTrail là *Regional Service*.
CloudTrail là service mà log lại các API actions trên AWS Account của chúng ta. Ví dụ:
- Start/Stop một EC2 instance.
- Thay đổi một Security Group.
- Create/Delete một S3.
Logs API calls/activities được gọi là CloudTrail Event. Nó là record của một hành động được thực hiện trên AWS Account.
Hành động này có thể được thực hiện bởi user, role, hoặc là một AWS service.
CloudTrail mặc định lưu trữ log lên tới 90 ngày trong Event History.
Event History được enable mặc định - Không tính phí trong 90 ngày đầu tiên.
Nếu muốn log được lưu trữ hơn 90 thì ta cần phải custom lại bằng cách *tạo mới*
một hoặc nhiều trail khác.
Có 3 loại trails cơ bản: *Management Event, Data Event, Insight Event*.
Management Event: cung cấp thông tin về các hoạt động quản lý mà được thực hiện trên resources trong AWS account. còn được gọi là *Control Plane Operation*. Ví dụ như tạo một EC2 instance, terminate EC2 instance, tạo VPC.
Data Event: cung cấp thông tin về resources operations trên một AWS resources. Thông tin về object đang được upload trên S3, object đang được truy cập từ S3, khi nào một Lambda Function được gọi.
Mặc định CloudTrail chỉ log Management Even vì Data Event thường chiếm volume lưu trữ rất lớn.
### Deep Dive
Ta có thể tạo trail cho một region cũng như tạo trail cho toàn bộ region.
Tạo trail thuộc một region thì trail đó chỉ ở region mà nó được tạo và nó chỉ log những event ở region mà nó được tạo ra.
All region trail là một collection của tất cả các trail ở mọi AWS region. Lợi ích là khi AWS thêm mới một region thì log event ở region đó cũng sẽ được tự động gửi lên trail này.

Khi ta tạo tạo một S3 bucket ở AP SouthEast 2 thì nó sẽ được logged ở region này. Và trail cần phải tạo ở region này hoặc có thể tạo all region trail để lắng nghe logging event này.

Có một số lượng nhỏ các service (***IAM, STS, CloudFront***) mà là globally service logged event globally tới ***US EAST 1 (VIRGINIA)***. Event này được gọi là Global Service Event, một trail cần phải được tạo ra để log các event này. Thường là nó sẽ được mặc định khi ta tạo trail.

Khi tạo trail thì management event sẽ được default pickup bởi trail, data event phải được ta manually enable.
Khi tạo trail thì nên nhớ rằng log trong trail được lưu trữ free 90 ngày dưới dạng *JSON*. Nếu có nhu cầu lưu trữ lâu hơn ta có thể dùng CloudTrail kết hợp với S3 Bucket, lúc này thì log data được lưu trữ mãi mãi, ta chỉ phải bỏ thêm tiền cho số lượng data được lưu trữ trong S3.
CloudTrait cũng có thể lưu trữ log trong CloudWatch Log. Khi log được lưu trữ trong CloudWatch Log ta có thể tìm kiếm hoặc sử dụng metric filter để thực hiện tracing, alarm,...

Ta cũng có thể AWS Organization Trail, trait này lưu trữ thông tin về api call, activities của tất cả account trong AWS Organization.

![[CloudTrail.png]]

### Summary
> [!CAUTION]
> Log trong CloudTrail mặc định được lưu trữ 90 ngày. Ta có thể lưu trữ log lâu hơn bằng cách sử dụng kết hợp với S3, CloudWatch Log.
> 
> *Trails* là một cách ta config lưu trữ log, nó có thể là one-region, all-regions. Có thể chứa Management Event (Enable Default) hoặc Data Event (Cần phải tự bật lên), có thể dùng kèm với S3,...
> 
> *IAM, STS, CloudFront* đây là những global service, nó mặc định log vào US East 1 (Virginia). Ta cần phải có 1 trait ở region này để lắng nghe log của những service này.
> 
> **NOT REAL TIME** CloudTrail log không phải là realtime log, thường thì activities sẽ mất khoảng 15 phút mới được log trong CloudTrail.

[[Control Tower]]