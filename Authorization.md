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
    Đây là nơi chứa cái thông tin mà được dùng để trả lời cho câu hỏi "Mã JWT được tính toán như thế nào?"
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
    Mục đích của nó được sử dụng là chứng mính răng dữ liệu được tạo ra bởi 1 nguồn xác thực. Dữ liệu chỉ được Encoded và Hash (Signed) chứ không phải Encrypted
#### Server xác thực mã JWT gửi lên từ client ra sao?
    Trong ví dụ trên ta sử dụng một chuối bí mật "secret" trong bươc tạo signature. Chuỗi "secret" này phải được lưu trữ cẩn thận ở phía server.
    Khi nhận được mã gửi Token gửi lên từ phía client, server sẽ lấy phần signature bên trong token đó, kiểm tra xem cái signature đó có đúng chính xác được HASH bởi cùng 1 thuật toán và chuỗi "secret" không.
    Cuối cùng,nếu signature đó khớp với chữ kí được tạo ra từ máy chủ, thì cái JWT đó là hợp lệ, ngược lại thì không, bên BE sẽ tùy từng trường hợp mà response về client một cách hợp lí
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
## Token, Refresh Token, Access token
### Token
    - Token chính là một thông tin rất cần thiết hay còn gọi là công cụ để truy cập giao diện tài nguyên (API)
    - Token bao gồm những gì: uid (danh tính duy nhất của người dùng), thời gian (dấu thời gian của thời gian hiện tại), ký hiệu (chữ ký, một vài chữ số đầu tiên của token được nén thành một chuỗi thập lục phân có độ dài nhất định bằng thuật toán Hashing hay còn gọi là băm)
    - Mỗi yêu cầu cần mang token và token cần được đặt trong header HTTP
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
    - Nếu bạn nghĩ rằng việc sử dụng cơ sở dữ liệu để lưu trữ Token sẽ khiến truy vấn mất quá nhiều thời gian, bạn có thể chọn lưu trữ trong bộ nhớ. 
### Refresh token
#### Định nghĩa
    Refresh token thực chất nó cũng chính là một token. Nhưng nó khác với Token Auth của JWT về chức năng đó là Refresh Token chỉ có một nhiệm vụ duy nhất đó là đề lấy một token mới, nêú token được cấp phát cho user hết hạn. Refresh token được cấp cho User cùng với token khi user xác thực đầu tiên nhưng thời gian của chúng khác nhau. Với token thì có thể 1 giờ, nhưng Refresh Token là có khi là 10 ngày.
#### Hoạt động
    Theo mô hình JWT thì:
    - Máy khách sử dụng username và password để xác thực.
    - Máy chủ tạo token JWT với thời gian hiệu lực ngắn hơn (VD:10p) và Refresh token với thời gian hiệu lực dài hơn (VD: 7 ngày)
    - Khi client truy cập vào giao diện cần yêu cầu xác thực, nó sẽ mang token theo
    - Nếu token chưa hết hạn, server sẽ trả về dữ liệu và khách hàng cần sau khi xác thực
    - Nếu xác thực thất bại khi truy cập vào giao diện yêu cầu xác thực bằng token (VD trả về lỗi 401), sử dụng refresh token để áp dụng cho token mới từ giao diện làm mới
    - Nếu refresh token chưa hết hạn, máy chủ sẽ cấp token mới cho khách hàng
    - Máy khách sử dụng token mới để truy cập vào giao diện yêu cầu xác thực
#### Refresh token được lưu ở đâu?
    Khác với token JWT được lưu ở Cookies, Session, hay localStorage thì Refresh Token được lưu ở database ở phía server (với tôi nó là vậy). Vì sao lại như vậy, vì Refresh Token sẽ không sử dụng cho việc khi máy khách yêu cầu trên giao diện, và nó chỉ được sử dụng khi một token hết hạn. Nó không ảnh hưởng đến hiệu suất của database, hay liên quan đến việc phản hồi từng yêu cầu của Client, nên nó không cần thiết phải lưu như một token bình thường
### Access token
    Access token là đoạn mã sinh ra ngẫu nhiên được sử dụng bí mật cho mỗi người dùng, ứng dụng khi thực hiện các thao tác quan trọng hay truy cập vào tài khoản của người dùng. 
    Trong trường hợp này, bạn có thể hiểu access token như một đường hầm bí mật để đi vào ngôi nhà của bạn. Các hình thức xác thực như username, password giống như khóa và chìa khóa cửa nhà của bạn. Access token sẽ không đi qua cánh cửa này. 
    Khi ai đó kết nối với một ứng dụng bằng hình thức đăng nhập facebook, ứng dụng đó có thể lấy access token cung cấp quyền truy cập tạm thời, an toàn vào API facebook. 
    Access token là chuỗi không rõ xác định người dùng, ứng dụng hoặc trang và ứng dụng có thể dùng mã đó để thực hiện lệnh gọi API và có thể lấy access token bằng nhiều phương thức khác nhau. 
## Tổng kết về token
    - Access token chỉ nên có giới hạn trong thời gian ngắn. Ví dụ như trong ngân hàng phiên làm việc rất ngắn hạn đảm bảo cho việc không bị tấn công qua token, token của client rất dễ bị lộ đa số storage như cookies...
    - Việc xác thực một accest token chỉ làm việc tại Resource Server. Nhưng khi xác thực một Refresh Token thì nó lại làm việc trên Authorization Server. Vì Authorization Server không những bắt buộc có một Refresh Token mà còn gửi cả password và username lên nữa mới có thể sinh ra một token mới.
    - Access token truy cập thường không được đặt quá lâu và refresh token là để kéo dài thời gian hiệu lực của mã thông báo truy cập. Việc tạo ra refresh token không phải để cho bạn thay thế điều như bạn nói, mà nó giúp hệ thống lấy lại được access token mới, mỗi khi nó hết hạn. Cũng như không cần phải lục lọi lại database để check thông tin khách hàng của bạn.
    - Quay lại cách mà accesss token làm việc thì, chúng ta thấy đa số là hết hạn tầm khoảng hai tiếng từ khi chúng sinh ra. Sau khi hết hạn bạn không thể để người dùng cứ 2 tiếng lại phải login một lần chứ? Trừ khi bạn cần họ, như banking internet vậy. Vì bạn cần bank, còn những trang như facebook bạn thử cho họ cứ 2 giờ login lại xem. Đảm bảo out ngay khi lập nghiệp đúng không? Chính vì vậy, cứ hết hạn server và lập trình viên phải uyển chuyển như thế nào để người dùng đang sử dụng mà vẫn không biết chuyện gì đang xảy ra. Trường hợp này chúng ta nên tìm hiểu về AXIOS INTERCEPTORS