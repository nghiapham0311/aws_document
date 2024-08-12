#### Object Versioning
Object versioning được kiểm soát ở bucket level.
Mặc định thì Object Versioning bị disable đi. Ta có thể tuỳ chọn enable tính năng này trên bucket. Nên lưu ý khi đã enable rồi thì không thể disable lại.
Ta chỉ có thể Suspended tính năng này thôi, và nó chỉ có thể chuyển trạng thái về enable lại. Không bao giờ về được disable.
![[Object Versioning & MFA Delete.png]]

**Nếu Object Versioning chưa được enable** lên thì mỗi Object được identify bởi Object Key, tức là tên của nó, unique trong bucket. Khi Object Versioning chưa được enable thì id của Object được set là null.
Nếu ta modified object thì phiên bản original sẽ bị replace đi và thay bằng phiên bản đã được update. 

Object Versioning cho phép ta lưu trữ nhiều phiên bản của một Object trong bucket.
Bất kỳ một operation nào modify một Object thì sẽ tạo ra một phiên bản mới của Object đó trong bucket và giữ nguyên phiên bản trước đó.

**Nếu ta bật Object Versioning** thì khi update một Object, AWS sẽ gán ID cho Object mới được update.
Giả sử ta modify object cũ bằng cách upload một Objet mới trùng tên với Object cũ thì khi này Object cũ vẫn không bị xoá đi, một Object mới được tạo ra với trùng tên nhưng ID khác nhau. Object mới được tạo ra sẽ được gọi là latest version. 
Nếu truy cập một Object mà không cung cấp cụ thể version nào thì latest version sẽ được return. Để get cụ thể version thì cung cấp cái Object ID thì sẽ get được.
![[Object Versioning & MFA Delete-1.png]]

**Object Version cũng ảnh hưởng tới việc xoá Object**. Ví dụ như ta request xoá một Object đã được enable Object Versioning trong bucket mà không cung cấp cụ thể version. Lúc này AWS sẽ tạo một version đặc biệt và gọi đó là *delete marker*, S3 không hề xoá Object ở trong bucket mà chỉ đơn giản tạo một version mới tên là delete market và hidden đi cái Object trên màn hình console.
Ta có thể *xoá delete marker* và Object sẽ trở lại state trước đó nơi có 1 Object là latest và các phiên bản của Object đó.
Để *xoá hoàn toàn một Object* thì ta cần phải cung cấp version cụ thể thông qua **Object ID** thì lúc này mới xoá được.
![[Object Versioning & MFA Delete-2.png]]

> [!CAUTION]
> - Object Versioning không thể bị disable sau khi đã enable, chỉ có thể suspend.
> - Sau khi bật Object Versioning thì tất cả các version của Object sẽ được lưu trữ trong bucket đó. Nếu Object A nặng 5GB và có 5 version, ta sẽ tiêu tốn 25GB.
> - Cách duy nhất để giảm cost là xoá bucket đi và upload lại Object lần này nhớ tắt Object Version.
> - Suspend vẫn bị tính tiền nha, vì mình vẫn store các version khác nhau của Object đó.

#### MFA Delete
Được enable lúc mình bật Object Versioning.
Lúc này MFA được bắt buộc phải có nếu ta change bucket versioning state. Change từ enable về suspend hoặc ngược lại thì phải cung cấp MFA.
MFA cũng được bắt buộc khi xoá một phiên bản của Object nằm trong bucket.

[[S3 Performance Optimization]]