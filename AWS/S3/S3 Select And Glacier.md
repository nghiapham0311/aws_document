Là một cách mình có thể retrieve một phần của Object thay vì toàn bộ Object.
#### Fundamental
- S3 có thể lưu trữ một số lượng lớn data với dung lượng của mỗi data có thể lên tới 5GB.
- Thường thì khi sử dụng S3 ta sẽ muốn GET luôn cái file nặng 5GB về luôn.
- Nếu mà mình lấy luôn cái file 5GB về thì chắc chắn nó sẽ tốn thời gian, và sử dụng 5GB transfer OUT of S3 cái này thì bị tính tiền.
- S3 Select/Glacier cho phép dùng câu SQL để Select một phần của Object được lưu trữ trong S3. Mình đã filter data, và mình chỉ tốn phần data này lúc transfer cho client không bị tính tiền toàn bộ Object.
- Cho phép select trên JSON, CSV,....
#### How they work
![[S3 Select And Glacier.png]]
Nếu filter ở application thì ta vẫn bị tính phí cho lượng data được transfer out khỏi S3, và performance sẽ không cao vì ta phải đọc một file có dung lượng rất lớn.

Sử dụng S3 Select/Glacier ta đặt filter ở trong S3 Service, filter những data cần được chuyển đi tới application. Lúc này ta chỉ bị charge cho phần data đã được filter đi.

[[S3 Event]]