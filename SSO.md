# SSO
## Định nghĩa, ưu nhược điểm
### Định nghĩa
    - SSO có nghĩa là Single Sign-On
    - Là cơ chế cho phép người dùng có thể truy cập nhiều trang web, ứng dụng mà chỉ cần một lần đăng nhập
    Ví dụ: Tài khoản Google có thể truy cập được nhiều trang web khác như youTube,Gmail,...Đây chính là cơ chế đăng nhập một lần. Khi bạn đăng nhập tài khoản lần đầu ở Google sẽ tự động chuyển đến trang chủ google để đăng nhập. Sau khi nhập xong username và password, Google sẽ gửi về một số cookie xác thực và chuyển hướng về Gmail. Sau đó nếu ta muốn đăng nhập vào YouTube, nó sẽ chuyển hướng ta về tài khoản Google, tài khoản này sẽ xác thực vs cookie xác thực ở trước đó và ta sẽ đăng nhập thành công bên YouTube
### Ưu nhược điểm
#### Ưu điểm:
    - Với một tài khoản ta có thể đăng nhập ở nhiều trang web thay vì phải nhớ rất nhiều tài khoản cho nhiều trang web khác nhau
    - Giảm được số lần đăng nhập 
    - Tỉ lệ lộ thông tin người dùng ít hơn
    - Tránh trường hợp người dùng quên tài khoản và mật khẩu do quá nhiều tài khoản cần nhớ
    - Trải nghiêm đăng nhập tốt hơn
#### Nhược điểm
    - Phải thông qua bên thứ 3
## Cơ chế hoạt động
### Hệ thống nhận dạng liên kết
    - Xác thực (Authentication): kiểm tra thông tin đăng nhập và định danh người dùng
    - Phân quyền (Authorization): Sau khi kiểm tra định danh xong ,dựa trên thông tin định danh để kiểm tra quyền truy cập user
    - Trao đổi thông tin người dùng (User attributes exchange): Các thông tin người dùng như tên, họ,... sẽ dễ bị trùng lặp. Các hệ thống con cần các thông tin này và phải lưu trữ chúng. Sẽ có một nơi tổng hợp lại các thông tin này và trao đổi với hệ thống con
    - Quản lí người dùng (User management): admin có thể quản lí hệ thống người dùng qua các hoạt động thêm sửa xóa
    => SSO là một phần của hệ thống nhận dạng liên kết, có liên hệ chặt chẽ với việc xác thực người dùng. Nó sẽ định danh người dùng rồi chia sẻ thông tin định danh đó vs các hệ thống con
### Cơ chế hoạt động
    - Chức năng của nó giống như store trong redux để cho các component lấy được các dữ liệu. Thì ở đây SSO sẽ tạo ra một domain trung tâm.
    - Domain trung tâm này sẽ lưu và chia sẻ các thông tin cookie cho các domain của những trang web lại với nhau
    - Từ domain trung tâm này, thông tin về cookie của các trang web sẽ được chia sẻ đến những domain con. 
    * Bạn có thể hình dung như cách sau:
    Khi bạn đăng nhập vào youtube(domain con) thì sẽ được điều hướng đến google(domain trung tâm). Token sẽ được google trả về và lưu ở trình duyệt. Tiếp đến bạn muốn truy câp vào những trang web khác(domain con) như gmail,ggphoto,.. thì cũng tương tự, nó sẽ điều hướng đến google(domain trung tâm), tuy nhiên do lần này đã có token từ trước nên k cần phải đăng nhập lại nữa
### Social login
    - Tương tự như giải thích ở trên thì social login cũng là 1 dang của Single Sign-On, là sử dụng thông tin đăng nhập trên các trang web lơn Google,FaceBook,...
    - Khi này domain trung tâm chính là domain của mạng xã hội. Nhờ đó ta có thể tạo tài khoản ở các domain con thông qua tài khoản của domain mạng xã hội.
### Luồng chạy chương trình của hệ thông sử dụng SSO vs Oauth
    B1: User vào domain1 để đăng nhập.
    B2: Domain1 sẽ chuyển hướng sang domain trung tâm
    B3: Người dùng đăng nhập và nhờ thông tin đăng nhập này, domain trung tâm sẽ kiểm tra và định danh người dùng
    B4: Domain trung tâm sẽ lưu vào Store Cookie đông thời chuyển hướng về domain1 kèm với 1 token
    B5: Domain 1 sẽ dùng token đó để xác thực định danh người dùng và lưu vào stores domain1 cookie
    B6: Nếu uset đăng nhập vào domain2.
    B7: Domain 2 sẽ chuyển hướng về domain trung tâm tương tự như domain 1. Nhưng khi này cookie đã có sẵn rồi nên doamin trung tâm sẽ trả về token và chuyển hướng về domain 2.
    B8: Domain 2 sẽ dựa vào token để xác minh và lưu thông tin vào stores domain 2 cookie
    => Tóm lại thì: 
    - domain con chuyển hương đến domain trung tâm cho việc xác minh. 
    - Người dùng đăng nhập của domain trung tâm. 
    - Domain trung tâm sẽ chuyển hướng về domain con đính kèm với một token được sinh ra ngẫu nhiên. 
    - Ở domain con, ta sẽ sử dụng token đó để call API tới domain trung tâm cùng vs ID và Secret Key tạo nên Access Token. 
    - Sau đó nhưng request từ client đều sẽ cần có thêm access token(như ở bài trước)
    
    