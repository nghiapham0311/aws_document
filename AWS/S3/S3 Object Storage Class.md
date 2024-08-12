#### S3 Standard
Đây là default storage class trong S3.
![[S3 Object Storage Class-1.png]]
Bob upload một hình ảnh sử dụng S3 PutObjectAPI, lúc này thì S3 Standard sẽ được sử dụng làm default storage class.
Hình ảnh sau khi upload sẽ được replicate ở 3 AZ khác nhau trong cùng region để đảm bảo 1 AZ fail thì hình ảnh vẫn có thể hiển thị tới end user.
Standard class có thể hiển thị data ngay lập tức (latency tầm miliseconds)

> [!CAUTION]
> Sử dụng S3 Standard cho việc lưu trữ những data cần access thường xuyên. Những data này thường rất quan trọng và không thể thay thế.
> 
> Nên dùng default, cái đầu tiên nghĩ tới. Chỉ khi có những nhu cầu đặc biệt thì mới tính tới các storage class khác.

#### S3 Standard Infrequent Access (S3 Standard IA)
![[S3 Object Storage Class-2.png]]
Tương tự như với S3 Standard. Dữ liệu vẫn sẽ được replicate trong ít nhất 3 AZ. Durability tương tự, Availability tương tự, Latency cũng tương tự.
Giá thì rẻ hơn một nửa so với S3 Standard.
Khác với Standard Class thì IA có phí retrieval khi ta truy cập vào những dữ liệu được lưu trong S3. Để đánh đổi với việc lưu trữ rẻ thì đổi lại chi phí lấy ra lớn.
Nếu không access 30 ngày thì nên lưu trữ kiểu này.

> [!CAUTION]
> Sử dụng S3 IA cho việc lưu trữ những data không cần access thường xuyên. Những data này thường rất quan trọng nhưng không truy cập thường xuyên. Ví dụ file log trên production, cứ lưu trữ đi, lâu lắm có issue thì móc ra coi.

#### S3 One Zone IA
![[S3 Object Storage Class-3.png]]
Rẻ hơn cả 2 thằng ở trên.
Các tính chất thì tương tự với Standard IA. Nhưng data sẽ không được replicate across AZ, mà chỉ store ở một AZ duy nhất.
Mình tận hưởng giá rẻ nhưng đổi lại phải tự chịu trách nhiệm cho việc mất mát data nếu AZ fail.

> [!CAUTION]
> Sử dụng S3 One Zone IA cho việc lưu trữ những data không cần access thường xuyên, không thay đổi thường xuyên. Những data này thường không quan trọng và không truy cập thường xuyên.

#### S3 Glacier Instant Retrieval
![[S3 Object Storage Class-4.png]]
Tương tự với S3 Standard IA. Ta lưu trữ dữ liệu không cần access thường xuyên ví dụ như là 1 lần 1 tháng.
Ta sử dụng loại này nếu như dữ liệu của ta còn ít nhu cầu access hơn nữa. Ví dụ như 1 quý 1 lần.
Loại này tăng thời hạn lưu trữ ít nhất để charge tiền là 90 ngày so với Standard IA.
Thời gian có thể để retrieve dữ liệu là *miliseconds*.
S3 Standard -> S3 Standard IA -> S3 Glacier Instan Retrival.
#### S3 Glacier Flexible
![[S3 Object Storage Class-5.png]]
Data vẫn sẽ được replicate across ít nhất là 3 AZ trong cùng 1 region.
Giá hiện tại là 1/6 so với S3 Standard.
Hãy nghĩ rằng data được lưu trữ sử dụng class này là cold data. Bởi vì là cold lạnh nên cần phải rã đông, điều đó có nghĩa là data sẽ không sẵn sàng để hiển thị ngay lập tức.
Bởi vì data bị cold rồi, ta cần rã đông. Quá trình này gọi là *Retrieval Process*, và ta sẽ bị tính tiền cho quá trình này. Có 3 loại Job Retrival cơ bản:
- Expedited xong trong khoảng 1 - 5 phút.
- Standard xong trong khoảng từ 3 tới 5 giờ.
- Bulk xong trong khoảng từ 5 tới 12 giờ.
**Class này sẽ bị delay data từ phút tới giờ.**
> [!CAUTION]
> Sử dụng cho việc lưu trữ những data không cần access thường xuyên, không cần real time access. Những data này thường một năm truy cập lần thôi. Và sẽ có delay trong việc access data trong khoảng thời gian từ vài phút tới vài giờ đồng hồ.

#### S3 Glacier Deep Archive
![[S3 Object Storage Class-6.png]]
Giá lưu trữ rẻ hơn thằng ở trên nữa. Kiến trúc tương tự luôn, chỉ khác biệt ở thời gian rã đông là từ **vài giờ đến vài ngày**.
Hãy nghĩ tới data được lưu trữ sử dụng class này ở trạng thái đóng băng mẹ rồi, tuyết rơi ở bắc cực. Cần búa tạ các thứ nung chảy nóng hết cỡ may ra lấy ra được, và điều này cần nhiều thời gian.
> [!CAUTION]
> Sử dụng cho việc lưu trữ những data hiếm khi access. Những data này thường vài năm truy cập lần thôi. Thường là Legal hoặc Regulation data.

#### S3 Intelligent Tiering
Chứa 5 storage tier khác nhau. Khi sử dụng class này thì sẽ có nhiều cách để store Object hơn. 
![[S3 Object Storage Class-7.png]]
Không cần lo lắng về việc luân chuyển data giữa các tầng, AWS sẽ lo hết cho chúng ta. Chi phí như cũ, nhưng sẽ có thêm phần chi phí để quản lý, tính tiền trên 1000 Object.
![[S3 Object Storage Class-8.png]]

[[S3 Lifecycle Configuration]]