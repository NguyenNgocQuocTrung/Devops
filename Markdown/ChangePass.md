---

title : "Change password of user by /etc/shadow"
date : "`r Sys.Date()`"
weight : 1
chapter : false

---
# <span style="color: red;">Chỉnh sửa password cho user bằng cách chỉnh sửa trong file /etc/shadow mà không dùng lệnh passwd </span>

**Password** trong file /etc/shadow được lưu dưới dạng băm (hash), sử dụng một trong các thuật toán mã hóa an toàn. Kiểu mật khẩu này không lưu trữ mật khẩu rõ ràng (plain-text) mà là chuỗi đã được băm và có thể được định dạng với các thuật toán mã hóa khác nhau.

- Mỗi chuỗi băm trong file /etc/shadow có một prefix để chỉ rõ loại thuật toán được sử dụng. Đây là một số dạng phổ biến:

### Các loại mã hóa trong file /etc/shadow
**SHA-512 (Prefix \$6\$):**

Đây là thuật toán mã hóa **SHA-512**, một trong những thuật toán mã hóa an toàn nhất hiện nay.
Ví dụ:

\$6\$random_salt\$hashed_password
\$6\$: Chỉ định SHA-512 là thuật toán được sử dụng.
random_salt: Phần salt để tăng tính bảo mật.
hashed_password: Mật khẩu đã được băm.

**SHA-256 (Prefix \$5\$):**

Sử dụng thuật toán SHA-256 để băm mật khẩu.
Ví dụ:

\$5\$random_salt\$hashed_password

**MD5 (Prefix \$1\$):**

Sử dụng thuật toán MD5 để băm mật khẩu, tuy nhiên MD5 không còn an toàn và ít được sử dụng.
Ví dụ:

\$1\$random_salt$hashed_password

**DES (Không có prefix hoặc prefix là \$2\$):

Đây là dạng mã hóa cũ, sử dụng thuật toán DES. Hiện tại, nó được coi là kém an toàn và hiếm khi sử dụng.
Ví dụ:

random_salthashed_password


- Mật khẩu mà chúng ta thấy trong file /etc/shadow thường thuộc dạng SHA-512 nếu prefix là $6$. Đây là một trong những chuẩn mã hóa hiện đại và an toàn, giúp đảm bảo rằng ngay cả khi mật khẩu bị lộ ra ngoài, hacker vẫn không dễ dàng có được mật khẩu thực mà phải trải qua quá trình bẻ khóa mật khẩu rất khó khăn.

- Vì vậy để có thể thay đổi password trong /etc/shadow. Chúng ta cần mã hóa đúng định dạng, đăng nhập user root và chỉnh suawr qua môi trường vim.

- Ở đây user test1 với password là 2 có định dạng mật khẩu như sau 

![Test1](/images/test1.png) 

- Mình sẽ thay đổi password thành 1, sử dụng openssl để tạo hash cho mật khẩu "1":

### openssl passwd -6 1

![Test1](/images/openssl_hash.png) 

- Sau đó đăng nhập root và thay đổi trong /etc/shadow thôi! Đúng không Mr.Hùng
