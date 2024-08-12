#### Fundamental
- Object Lock có thể bật trên new bucket (Với những bucket cũ thì liên hệ AWS Support), khi sử dụng thì cần phải bật Object Version.
- Khi bật Object Lock thì không thể disable hoặc suspending versioning nữa.
- Object Lock implement theo kiểu Write-One-Read-Many (WORM) - Không có Delete, Không có Overwrite.
- S3 Bucket có thể config default Object Locking.
Có 2 cách để quản lý Object Retention
- Retention Period
- Legal Hold
Một Object version có thể có 1, 2 hoặc không cái nào.
#### S3 Object Lock Retention
Khi tạo Object Lock thì cung cấp luôn khoảng thời gian mà Object này bị lock, có thể là ngày/tháng/năm. Một năm có nghĩa là nó sẽ end tính từ ngày bị lock tới 1 năm sau.
##### Compliance Mode
Nếu set Object Lock theo mode này có nghĩa là Object Version không thể adjusted, deleted, overwritten trong khoảng thời gian retention period.
Nó cũng có nghĩa là retention period không thể giảm, và retention mode không thể thay đổi được trong khoảng thời gian retention period. Kể cả root user luôn cũng không làm gì được.
Lý do sử dụng: Luật yêu cầu giữ lại data về y tế, bảo hiểm trong thời gian 3 năm. Không một thằng nào được chỉnh sửa, xoá data gì hết nghe.
##### Governance
Tương tự như trên không thể giảm retention period, không thể thay đổi mode trong retention period.
Nhưng có thể cấp một số quyền đặc biệt cho các identities để by pass mode này.
#### S3 Object Lock Legal Hold
Sử dụng loại này thì không cần set thời gian retention period.
Set on Object Verion On hoặc Off.
Khi bật lên thì Object Version không được xoá, hoặc sửa.
s3:putObjectLegalHold là permissions được requires để thêm hoặc xoá.
Tác dụng ngăn chặn việc có thể xoá nhầm những critical version của một Object.
#### Summary

![[S3 Object Lock.png]]
[[S3 Access Point]]