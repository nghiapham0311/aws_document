#### Definition
Pre-signed URL là một cách mình cho application, người user khác quyền access S3 Object trong S3 Bucket sử dụng credential của mình một cách secure và safe.
#### How they work
![[S3 Presigned URL-1.png]]
S3 mặc định sẽ block hết public access.
Điều này có nghĩa là để truy cập S3 Object trong S3 Bucket thì phải đăng nhập và chứng tỏ quyền hạn của bản thân thì mới có quyền truy cập vào S3 Bucket này.
Bob cần truy cập vào S3 Bucket để lấy S3 Object, nhưng lại không có AWS Account cũng như không được cấp quyền hạn.
Các option hiện tại: Mở public S3 Bucket, Tạo AWS Identity và phân quyền cho Bob. Nhưng những option này cũng không còn phù hợp vì Bob chỉ cần truy cập vào S3 Bucket trong một khoảng thời gian rất ngắn.

![[S3 Presigned URL-2.png]]
Cách giải quyết bài toán ở trên đó chính là pre-signed url. iamadmin sẽ cung cấp AWS Credential cho S3 để generate ra pre-signed url. pre-signed url chứa các thông tin về S3 Bucket nào được truy cập, S3 Object nào được access và sẽ expire trong một khoảng thời gian.
Sau đó iamadmin đưa pre-signed url này cho bất kì người nào có nhu cầu tương tác với S3.
Lưu ý khi đưa url xong thì người sử dụng cũng chính là iamadmin, vì url đó mang quyền hạn của iamadmin để tương tác với S3. iamadmin làm được gì với S3 thì người có url cũng làm được vậy.

![[S3 Presigned URL-3.png]]


> [!CAUTION] Đi thi
> - Có thể tạo pre-signed url cho S3 Object mà mình không có quyền truy cập.
>   
> - Khi sử dụng pre-signed url, thì permissions của cái url trùng với permissions của người generated ra cái url này hiện tại luôn.
>   
> - Nếu bị access denied nó có nghĩa là thằng identity không có access hoặc trước kia có access rồi mà hiện tại thì không có.
> 
> - Đừng generate pre-signed url sử dụng role. Như ta biết role chỉ tồn tại trong một khoảng thời gian. URL sẽ dừng hoạt động ngay khi cái temporary credentials khi assume cái role expire đi.

[[S3 Select And Glacier]]
