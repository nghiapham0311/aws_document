### Fundamental
Thường thì ta sẽ truy cập S3 thông qua AWS API.
Sử dụng API đang lại một số lợi ích đáng kể. Bởi vì nó bảo mật và flexible.
Sử dụng S3 làm static hosting cho phép truy cập thông qua *http*. Do đó nó cho phép ta host mọi thứ. Ví dụ như media, một cái blog đơn giản.
Sử dụng tính năng này khá đơn giản, enable lên, cung cấp cho 2 page *index và error*. Point index document vào một object cụ thể trong S3.
Khi ta bật tính năng này thì một *Website Endpoint* sẽ được AWS tự động tạo ra. Tên của endpoint chính là tên của bucket kết hợp với region mà bucket này được tạo ra.
Ta có thể sử dụng custom domain, nhưng nhớ để ý rằng tên của domain phải match với tên của bucket. Ví dụ domain name là minhducitus.com thì bucket name cũng phải là minhducitus.com.

### Use case
![[S3 Static Hosting.png]]
##### Offloading
Giả sử ta có một static website được render dynamic từ EC2 instance.
Lúc này ta có thể sử dụng static website hosting để host những file image, media.
Khi đó EC2 chỉ render ra html bình thường, ở những thẻ image sẽ point về S3 bucket có chứa object tương ứng.
Việc này dẫn tới offloading cho việc loading content của EC2.
##### Out of band pages
Giả sử như application của ta đang chạy trên EC2 instance thì bị dính bug, hoặc server sẽ tạm thời down trong khoảng thời gian maintenance.
Lúc này ta có thể sử dụng static website hosting để đưa lên một trang thông báo cho end user về việc server đang gặp sự cố. Khi user cố truy cập vào EC2 thì ta sẽ routing về page báo lỗi này.
Tất nhiên là trang này sẽ không nằm trong EC2 instance vì server đang bảo trì mà đưa vào chưa chắc gì nó đã work.

[[Object Versioning & MFA Delete]]
