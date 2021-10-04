# Authorization
## JWT và Oauth2
### JWT
#### Hoàn cảnh ra đời
    Thay thế cho mô hình Session - Cookie vì sự hạn chế của mô hình này đối vs ứng dụng di động. Vì ứng dụng di động không có Cookie.
#### Định nghĩa
    JWT là một JSON object được định nghĩa trong chuẩn RFC 7519 như là một cách an toàn để trao đổi thông tin giữa hai bên. Và Token thì bao gồm một header, một payload và một chữ ký
#### Cơ chế hoạt động
    User sẽ login với {username, password...}. Dữ liệu này sẽ được gửi lên Server. Server sẽ tạo một mã JWT và sau đó gửi về cho phái client lưu trữ. Những request tiếp theo từ client gửi lên thì phải đính kèm mã JWT này(thông thường thì ở Header), server sẽ check mã này và gửi lại response thành công hay thất bại tương ứng ngược về client.
#### Cách tạo một mã JWT
    JWT bao gồm 3 thành phần quan trọng: HEADER, PAYLOAD, SIGNATURE
##### HEADER
    Đây là nơi chứa cái thông tin cách biển đổi token"
    Ví dụ
```JSON
    {
        "typ":"JWT",
        "alg":"HS256"
    }
```
    Ở đây typ là viết tắt cho type là kiểu token, ở đây là JWT. Còn alg là thuật toán sử dụng tạo ra chữ kí cho Token. Ở đây là thuật toán HS256(HMAC-SHA256), một thuật toán sử dụng khóa bí mật để tính toán tạo ra chữ kí
##### PAYLOAD
    Đây là nơi chứa dữ liệu mà chúng ta muốn lưu lại trong JWT
    Ví dụ:
```JSON
    {
        "userID":"7j79y-kdjr8n4h-5jd8-5k39-cfk8ghr9wu",
        "username": "hoanganh9899",
        "occupation": "FE dev",
        // standard fields
        "iss": "Le Hoang Anh",  /// thông tin người tạo token
        "iat": 1568456819,   /// thới gian lúc token được tạo
        "exp": 1568460419    /// thời gian lúc token hết hạn
    }
```
##### SIGNATURE
    Được hiểu là chữ kí cho client. Chữ kí này được tạo ra tùy theo thuật toán mà BE muốn tạo cho JWT. Ví dụ:
```javascript
    const headerEncode = base64urlEncode(header); // ví dụ kết quả: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9
    const payloadEncode = base64urlEncode(payload); // ví dụ kết quả: eyJ1c2VySWQiOiJiMDhmODZhZi0zNWRhLTQ4ZjItOGZhYi1jZWYzOTA0NjYwYmQifQ
    const data = headerEncode + "." + payloadEncode;
    const hashedData = Hash(data, secret);
    const signature = base64urlEncode(hashedData);  // ví dụ kết quả: xN_h82PHVTCMA9vdoHrcZxH-x5mb11y1537t3rGzcM
    // cuối cùng thì mã JWT theo đúng cấu trúc header.payload.signature sẽ trông như sau:
    const JWT = headerEncode + "." + payloadEncode + "." + signature;
    // Kết quả: 
    "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VySWQiOiJiMDhmODZhZi0zNWRhLTQ4ZjItOGZhYi1jZWYzOTA0NjYwYmQifQ.xN_h82PHVTCMA9vdoHrcZxH-x5mb11y1537t3rGzcM"
```
#### JWT bảo vệ dữ liệu người dùng bằng cách nào?
    JWT không bảo vệ dữ liệu của bạn.
    JWT được biến đổi chứ vẫn có thể bị hack được. Vẫn có rất nhiều lần Facebook bị đánh cắp thông tin người dùng
#### Server xác thực mã JWT gửi lên từ client ra sao?
    Trong ví dụ trên ta sử dụng một chuối bí mật "secret" trong bươc tạo signature. Chuỗi "secret" này phải được lưu trữ cẩn thận ở phía server.
    Khi nhận được mã gửi Token gửi lên từ phía client, server sẽ lấy phần signature bên trong token đó, kiểm tra xem cái signature đó có đúng chính xác được HASH bởi cùng 1 thuật toán và chuỗi "secret" không.
    Cuối cùng,nếu signature đó khớp với chữ kí được tạo ra từ máy chủ, thì cái JWT đó là hợp lệ, ngược lại thì không, bên BE sẽ tùy từng trường hợp mà response về client một cách hợp lí
#### Tổng kết
    - JWT là JSON Web Token
    - Các thông tin trong chuối JWT được định dạng kiểu JSON
    - Chuỗi token cần phải có 3 phần : Header, Payload, Signature
    - Khi người dùng đã đăng nhập thành công thì những request tiếp theo từ client đều phải chứa thêm mã JWT, điều này cho phép người dùng truy cập được một số chức năng cấp phép
### Oauth2
#### Oauth là gì???
    OAuth là một phương thức chứng thực giúp các ứng dụng có thể chia sẻ tài nguyên với nhau mà không cần chia sẻ thông tin username và password. Từ Auth ở đây mang 2 nghĩa:
        - Authentication: xác thực người dùng thông qua việc đăng nhập.
        - Authorization: cấp quyền truy cập vào các Resource.
    Có thể thấy với một tài khoản Facebook bạn có thể đăng nhập được rất nhiều ứng dụng khác.
#### Oath2
    Sau nhiều bản Oauth1 ra đời nhưng không hoàn thiện ở một số tính năng, dễ bị chiếm đoạt tài nguyên người dùng thì năm 2012, Oauth2 ra đời.
    Tuy vẫn còn lỗi bảo mật như dùng Chrome để hack Facebook nhưng hiện nay vẫn được sử dụng rộng rãi
    Oauth2 không đơn thuần chỉ là giao thức kết nối, nó là một "nền tảng" mà chúng ta phải triển khai ở cả hai phía: Client và Server
##### Các tác nhân đối tượng trong Oauth2
    Oauth2 làm việc với 4 đối tượng mang những vai trò riêng:
        - Resource Owner(User): Là những người dùng ủy quyền cho ứng dụng cho phép truy cập tài khoản của họ. Sau đó ứng dụng được phép truy cập vào những dữ liệu người dùng nhưng bị giới hạn bởi những phạm vi (scope) được cấp phép. 
        - Client(Application): Là những ứng dụng mong muốn truy cập vào dữ liệu người dùng. Trước khi được phép tương tác với dữ liệu thì ứng dụng này phải qua bước ủy quyền của User, và phải được kiểm tra xác nhận thông qua API. => Có thể hiểu là các ứng dụng sử dụng Facebook, Twitter, Google API chẳng hạn.
        - Resource Server (API): Nơi lưu trữ thông tin tài khoản của User và được bảo mật.
        - Authorization Server (API): Làm nhiệm vụ kiểm tra thông tin user (VD: ID), sau đó cấp quyền truy cập cho Application thông qua việc phát sinh "access token".
    Resource Server và Authorization Server chính là điểm khác biệt cơ bản giữa OAuth2 và OAuth1
##### Luồng hoạt động
    - Application yêu cầu ủy quyền để truy cập vào Resource Server thông qua User
    - Nếu User ủy quyền cho yêu cầu trên, Application sẽ nhận được giấy ủy quyền từ phía User dưới dạng một token string nào đó
    - Application gửi thông tin định danh (ID) của mình kèm theo giấy ủy quyền của User tới Authorization Server.
    - Nếu thông tin định danh được xác thực và giấy ủy quyền hợp lệ, Authorization Server sẽ trả về cho Application "access_token". Đến đây quá trình ủy quyên hoàn tất.
    - Để truy cập vào tài nguyên (resource) từ Resource Server và lấy thông tin, Application sẽ phải đưa ra "access_token" để xác thực.
    - Nếu "access_token" hợp lệ, Resource Server sẽ trả về dữ liệu của tài nguyên đã được yêu cầu từ Application.
    * VD khi bạn muốn đăng nhập instagram bằng tài khoản google. Thì đầu tiên instagram sẽ dẫn bạn đên trang web của Google và liệt kê một số quyền trang web đó muốn bạn cho phép. Khi này Google sẽ gửi một đoạn code cho bạn thông qua điện thoại hoặc qua mail. Và khi bạn nhập đúng mã code thì dựa vào id của bạn và code này nếu nhập đúng thì instagram sẽ trả về cho bạn một access token. Để sử dụng những chức năng của instagram thì khi bạn gửi resquest, nó sẽ đính kèm thêm access token này để xác thực. Nếu xác thực thành công thì instagram sẽ trả về các dữ liệu,giao diện bạn cần dùng
## Token, Refresh Token, Access token
### Token
    - Token giống như một chìa khóa, chữ kí để truy cập giao diện tài nguyên (API)
    - Mỗi token ngẫu nhiên, và chỉ được sử dụng 1 lần
    - Token bao gồm những gì: uid (danh tính duy nhất của người dùng), thời gian (dấu thời gian của thời gian hiện tại), ký hiệu (chữ ký, một vài chữ số đầu tiên của token được nén thành một chuỗi thập lục phân có độ dài nhất định bằng thuật toán Hashing hay còn gọi là băm)
    - Mỗi yêu cầu request từ client cần mang token và token cần được đặt trong header HTTP
    - Xác thực người dùng dựa trên token là phương thức xác thực không trạng thái trên Server. Server không cần lưu trữ dữ liệu token. Do đó giảm áp lực cho Server và giảm query liên tục và thường xuyên dưới Database
    - Token được quản lý hoàn toàn bởi ứng dụng, vì vậy nó có thể bỏ qua chính sách CORS
    - Thường thì khi client được return về token thì sẽ lưu ở hai chỗ phổ biến đó là Cookies và localStorage. Ngoài ra còn có sessionstorage và indexDB nhưng sessionstorage và indexDB thì ít được sử dụng
#### Token gửi từ client theo cách nào?
    Cách 1: Đặt trên header HTTP mỗi khi request:
        GET /application/v1/events
        Host: api.example.com
        Authorization: Bearer
    Cách 2: Chuyền qua URL
        "http://www.example.com/user?token=xxxxxxx"
    Cách 3:Khi tên miền chéo, bạn có thể đặt TOKEN trong phần body data như kiểu POST.
#### Lưu ý khi sử dụng token
    - Điện thoại di động không hỗ trợ cookie tốt và vì vậy Token thường được sử dụng trên những ứng dụng trên mobile.
    - Token được quản lý hoàn toàn bởi ứng dụng, vì vậy nó có thể bỏ qua chính sách CORS
### Refresh token
#### Định nghĩa
    Refresh token thực chất nó cũng chính là một token. Được tạo ra cùng với access token nhưng thời gian hiệu lực lâu hơn, chức năng là đề lấy một token mới, nêú access token được cấp phát cho user hết hạn. Khi refresh token hết hạn thì ta cần đăng nhập lại để nó tạo ra refresh token mới nhưng thường thì refresh token sẽ có giá trị sử dụng rất lâu nên ít khi xảy ra trường hợp này. Và khi người dùng logout thì resfresh token cũng sẽ bị xóa đi.
#### Hoạt động
    Theo mô hình JWT thì:
    - Đầu tiên user sẽ đăng nhập vs username và password. Sau đó client sẽ gửi username và password lên server
    - Server tạo token JWT với thời gian hiệu lực ngắn (VD:10p) để tránh bị hack token và Refresh token với thời gian hiệu lực dài hơn (VD: 7 ngày). 
    - Khi client truy cập vào giao diện, gửi yêu cầu request, thì trong request cần mang token theo
    - Nếu token chưa hết hạn, server sẽ trả về dữ liệu và khách hàng cần sau khi xác thực
    - Nếu xác thực thất bại hoặc token đã hết hạn (VD trả về lỗi 401), thì refresh token sẽ được gọi để tạo ra một token mới thay cho token đã hết hạn
    - Máy khách sử dụng token mới để truy cập vào ứng dụng và sử dụng các chức năng của ứng dụng đó
    - Khi người dùng logout thì refresh cũng bị xóa theo
#### Refresh token được lưu ở đâu?
    Khác với token JWT được lưu ở Cookies, Session, hay localStorage thì Refresh Token được lưu ở database ở phía server. Vì Refresh Token sẽ không sử dụng cho việc khi user sử dụng các chức năng, và nó chỉ được sử dụng khi một token hết hạn. Và khi người dùng logout thì nó sẽ bị xóa theo luôn.
### Access token
    Access token là đoạn mã để xác thực quyền truy cập, cho phép bên thứ 3 truy cập vào những dữ liệu của người dùng trong phạm vi nhất định người người dùng ủy quyền cho phép. Access token này sẽ được đính kèm mỗi khi client gửi yêu cầu request lên server để truy cập đến những tài nguyên trong Resource server. 
    Access token thường sẽ có thời gian hiêu lực ngắn, vì hacker có thể dùng nhưng access token này để đánh cắp dữ liệu người dùng, nhưng nếu cứ một thời giàn ngắn phải đăng nhập lại thì quá bất tiện nên refresh token được sử dụng để tránh vấn đề này, nó sẽ tạo ra token mới cho người dùng sử dụng tiếp mà k cần phải nhập thông tin hay đăng nhập lại.
## So sánh access token và refresh token
    Access token gọi là token để truy cập còn refresh token là token để làm mới. 
    2 token này có thời gian sử dụng khác nhau, access token thì thời gian hiệu lực ngắn còn refresh token có thời gian hiêu lực rất dài có thể là cả tháng. 
    Refresh token có chức năng tạo mới token khi access token cũ bị hết hạn. 
    Refresh token được lưu ở server còn access token được lưu ở client
    Khi client gửi request thì phải đính kèm access token. Còn Refresh Token sẽ không sử dụng cho việc khi user sử dụng các chức năng
    2 token này hỗ trợ cho trong trong quá trình sử dụng của người dùng nhưng chức năng thì không hề liên quan đến nhau
## Tổng kết về token
    - Access token chỉ nên có giới hạn trong thời gian ngắn. 
    - Việc xác thực một accest token chỉ làm việc tại Resource Server. Còn Refresh Token thì nó lại làm việc trên Authorization Server. Vì Authorization Server không những bắt buộc có một Refresh Token mà còn gửi cả password và username lên nữa mới có thể sinh ra một token mới.
    - Tùy vào nhu cầu sử dụng mà ta sẽ dùng refresh token cho hợp lí, các app banking thì ko nhất thiết phải dung refresh token vì yêu cầu của nó cần người dùng đăng nhập lại thường xuyên tránh trường hợp bị đánh cắp tài sản. Còn với trường hợp đăng nhập bình thương như khi sử dụng facebook thì ta cần sử dụng đồng thời 2 token để tránh gây trải nghiệm xấu đến người dùng. 2 token này mỗi token 1 nhiệm vụ và nó hỗ trợ cho nhau trong quá trình sử dụng