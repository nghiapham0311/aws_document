##### Definition
Là service được khá nhiều AWS Service sử dụng trong việc encryption. Cũng giống như mọi AWS Service khác, ta cần permissions để access KMS.
KMS là regional và public service. Mỗi region được isolated với các region khác khi sử dụng KMS.
KMS cho phép ta Create/Store/Manage cryptographic key. Những cái key này dùng để convert simple text thành cipher và ngược lại.
KMS có thể handle *Symmetric* và *Asymmetric* key.
KMS cũng thể encrypt, decrypt,...
Những cái key này **không thể rời khỏi** KMS. KMS đạt tiêu chuẩn **FIPS 140-2 (L2)**.
##### KMS Keys
Là Key được KMS lưu trữ và sử dụng. Ta có thể sử dụng nó, application có thể sử dụng nó, AWS có thể sử dụng nó để encrypt, decrypt.
KMS Key (Logical) bao gồm ID, Date, Policy, Description, State.
KMS Key (Physical) là data được giữ bởi KMS và đây chính là thứ thật sự được dùng cho việc encrypt, decrypt.
KMS có thể có encrypt, decrypt file size lên đến *4KB*.
##### How KMS Work
![[S3 Key Management Service (KMS).png]]
Ashley sau khi chọn được AWS Region sẽ tạo KMS Key.
KMS Key được tạo ra với mọi thông tin cần thiết và Key này được lưu trữ trong KMS Service dưới dạng Encrypt. Không có thứ gì trong KMS Service mà store dưới dạng plain, tất cả đều là Encrypt.
Ashley có data cần được encrypt, cô ấy yêu cầu KMS encrypt data, cung cấp cho KMS Service cái Key nào nên dùng để thực hiện và cuối cùng là cung cấp data cần được encrypt. Đương nhiên Ashley phải có quyền Encrypt data thì mới làm được điều này. Sau đó KMS Service decrypt cái Key dùng để encrypt data, và dùng cái Key sau khi đã decrypt để encrypt data được cung cấp bởi Ashley và trả về data cho cô ấy.
Sau đó, Ashley có nhu cầu decrypt data, lúc này Ashley chỉ cần cung cấp data cần được Decrypt, không cần cung cấp thông tin về Key. Vì thông tin về Key đã được mã hoá bên trong dữ liệu được encrypt trước đó rồi. Đương nhiên Ashley cũng cần có permission để Decrypt. Sau đó thì tương tự với Encrypt.
Trong toàn bộ quá trình thì **KMS Key** sẽ **không** rời khỏi KMS Service. Ở mỗi step thì đều cần cung cấp permission cụ thể thì mới làm được (permissions tạo key, encrypt, sử dụng key, decrypt).
##### Data Encryption Key (DKEs)
Là một loại Key khác mà KMS Service có thể generate ra.
Generate KMS Key xong sử dụng GenerateDataKey Operation thì sẽ tạo ra được Key có thể dùng encrypt, decrypt data với dung lượng lớn hơn 4KB.
DEKs sẽ được link với KMS Key mà tạo ra nó.
KMS Service không lưu trữ DEKs, KMS Service sẽ dung cấp DEKs cho ta hoặc AWS Service sử dụng rồi sau đó discard cái Key này. Bởi vì KMS không thực hiện việc encrypt, decrypt mà là AWS Service sẽ làm việc này

> [!CAUTION]
> KMS Service là regional service nên Key sẽ được store ở region và không bao giờ rời khỏi cái region này cũng như KMS Service. Ta không thể extract một KMS Key ra ngoài.
> 
> Có AWS Owned Key và Customer Owned Key, đừng lo về AWS Owned Key, nó để cho AWS Service chạy thôi.
> 
> Trong Cusomter Owned Key cũng chia làm 2 loại là AWS Own và Customer Own.
> AWS Own ở đây chính là Key được AWS tạo ra tự động khi ta tương tác với AWS Service ví dụ như là S3.
> Customer Key ở đây là Key được ta tạo ra để sử dụng trong application hoặc AWS Service. Customer Key thì flexible hơn, có thể edit policy cho phép cross account access.
> 
> KMS hỗ trợ rotation.
> Có thể tạo alias.
> Backing Key Compatible, có thể dùng Key mới để decrypt data được encrypt bởi key cũ.

##### Key Policy & Security
![[S3 Key Management Service (KMS)-1.png]]
Key Policy là một loại Resource Policy.
Mỗi key cần phải có Principal mà nó tin tưởng và việc này cần phải config. Nó không tự động tin tưởng một cái gì hết.
Sử dụng kết hợp Key Policy + IAMPolicy để tăng cường security như ở json bên dưới. Cần có 2 cái json này thì mới chạy được.
Key phải tin tưởng Principal và Principal có quyền sử dụng Key để encrypt, decrypt.

```bash
echo "find all the doggos, distract them with the yumz" > battleplans.txt

aws kms encrypt \
    --key-id alias/catrobot \
    --plaintext fileb://battleplans.txt \
    --output text \
    --query CiphertextBlob \
    | base64 --decode > not_battleplans.enc 
    
aws kms decrypt \
    --ciphertext-blob fileb://not_battleplans.enc \
    --output text \
    --query Plaintext | base64 --decode > decryptedplans.txt
```

[[S3 Object Encryption CSE SSE]]
