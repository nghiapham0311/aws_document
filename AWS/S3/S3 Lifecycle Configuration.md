#### Fundamental
Mình có thể tạo ra những cái rule trên S3 Bucket có thể tự động chuyển đổi trạng thái expire của Object trong bucket.
- A Lifecycle Configuration là tập hợp của các rule.
- Rule gồm nhiều action. Ví dụ như làm X nếu Y đúng.
- Rule có thể apply cho toàn bộ Bucket hoặc một group of object trong bucket.
- Action gồm 2 loại
	-  Transition Action: chuyển đổi storage class của bucket. Đổi trạng thái của Object từ Standard sang Standard IA sau 30 ngày không có truy cập.
	- Expiration Action: ***XOÁ*** Object đi. Ví dụ xoá Object đi sau 30 ngày không có ai truy cập.
#### Deep Dive
Sử dụng Lifecycle như là một thác nước, dựa trên tần suất truy cập vào Object.
![[S3 Lifecycle Configuration-1.png]]

> [!CAUTION] Đi thi
> Nên chú ý với những Object có size nhỏ, bởi vì có giới hạn size giữa các storage class. Nhỏ hơn lưu tốn tiền hơn.
> 
> Một Object nếu được upload lên và sử dụng Lifecycle Configuration cần ở trong Standard class ít nhất là 30 ngày rồi mới được chuyển tới class tiếp theo. Ta có thể config Object store ở class tiếp theo luôn, nhưng nếu dùng Lifecycle thì cần phải đợi 30 ngày.
> 
> Single Rule không thể move một Object từ Standard IA về One Zone IA rồi về Glacier trong 30 ngày được. Data phải ở trong class đó 30 ngày rồi mới chuyển tiếp xuống. Nếu làm với nhiều rule thì có thể move sớm hơn 30 ngày được.

[[S3 Replication]]



