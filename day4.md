# Functional Programming (FP)
## Định nghĩa, đặc điểm
### Functional Programming là gì?
    Functional Programming nghĩa là “lập trình hàm” hay còn được gọi là “lập trình chức năng”. Đây là một phương pháp xây dựng phần mềm bằng cách tạo ra các chức năng, dựa trên các hàm toán học.
    Functional Programming sử dụng các các biểu thức và khai báo thay vì thực thi các câu lệnh. Chính vì vậy, cách lập trình này hoàn toàn khác với các thủ tục khác thường dựa vào những trạng thái cục bộ hoặc toàn cục. Giá trị đầu ra của Functional Programming chỉ phụ thuộc vào các tham số được truyền cho hàm
    JavaScript là một trong những ngôn ngữ nổi bật nhất của Functional Programming
### Đặc điểm của Functional Programming
    - Đây là phương pháp lập trình chú trọng kết quả, chứ không phải quá trình.
    - Nó nhấn mạnh vào những gì sẽ được tính toán
    - Dữ liệu là bất biến, không thể thay đổi
    - Phương pháp này biến các vấn đề cần giải quyết thành những chức năng
    - Nó được xây dựng dựa trên khái niệm về các hàm toán học. Cụ thể, nó sử dụng các biểu thức điều kiện và đệ quy để thực hiện phép tính
    - Nó không hỗ trợ việc lặp lại, như các câu lệnh lặp và câu lệnh điều kiện If-Else
## Ưu và nhược điểm
### Ưu điêm:
    - Nó cho phép bạn tránh được các vấn đề khó hiểu và lỗi trong mã
    - Người dùng dễ dàng thực hiện kiểm thử nói chung, đặc biệt là kiểm thử đơn vị (Unit testing) và gỡ lỗi mã (debug)
    - Ứng dụng xử lý song song và đồng thời
    - Hỗ trợ triển khai mã nóng và khả năng chịu lỗi tốt
    - Hỗ trợ các hàm lồng nhau
    // đoạn này e chưa hiểu ạ=))))
    - Tăng hiệu suất cho nhà phát triển
    - Cung cấp mô-đun tốt hơn đối với những đoạn mã ngắn
    - Hỗ trợ các cấu trúc dữ liệu hàm như Lazy Map (Bản đồ lười), Danh sách (List),…
    - Cho phép sử dụng hiệu quả Phép tính Lambda
### Nhược điểm:
    - Khó hiểu đối với người mới bắt đầu
    - Khó bảo trì vì có nhiều đối tượng phát triển trong quá trình viết mã
    - Yêu cầu nhiều ở quá trình bắt chước (mocking) và khởi tạo môi trường.
    - Việc tái sử dụng mã rất phức tạp và cần cấu trúc lại mã liên tục.
    - Các đối tượng có thể không đại diện chính xác cho vấn đề cần xử lý
## Một số thuật ngữ trong Functional Programming
    - Pure function(Hàm thuần khiết)
    - Function composition(Sự kết hợp các hàm với nhau)
    - Avoid shared state(Tránh chia sẻ biến)
    - Avoid mutating state(Tránh thay đổi biến)
    - Avoid side effects(Tránh hiệu ứng phụ)
### Pure Function
    - Luôn trả về kết quả giống nhau khi tham số truyền vào giống nhau. Nó không bị phụ thuộc bởi bất cứ trạng thái, dữ liệu hay thay đổi nào khi chương trình chạy mà chỉ phụ thuộc duy nhất vào tham số truyền vào
    - Không tạo ra những side effect đến chương trình ví dụ như gửi request, hay thay đổi dữ liệu bên ngoài phạm vi hoạt động của nó
```javascript
    function binhphuong(x){
        return x*x;
    }
```
    Hàm này không bị phụ thuộc vào biến bên ngoài, thay đổi dữ liệu hay sản sinh bất cứ side effect nào do đó hàm này được coi là pure. Nếu bạn chạy hàm này 1 triệu lần với tham số đầu vào không đổi ví dụ x là 10 thì cả 1 triệu lần nó sẽ đều trả về cùng một kết quả là 100.
    Nhưng nếu ví dụ như này:
```javascript
    var x = 10;
    function tinhtoan(a){
        return a*x/10;
    }
```
    Hàm này vi phạm một điều đó trong định nghĩa về pure function đó là nó bị phụ thuộc vào biến x bên ngoài nó, cho nên nó không thể coi là pure
### Function composition
    Đó là một tiến trình kết hợp hai hay nhiều hàm để tạo nên hàm mới hoặc thực hiện một nhiệm vụ gì đó.
### Immutability - Bất biến
#### Share state
    - Khái niệm: Là bất cứ các biến, đối tượng, hoặc không gian bộ nhớ mà chúng tồn tại trong một pham vi được chia sẻ(shared scope)
    - Một shared scope bao gồm scope toàn cục và scope closure. Chúng được dùng chung quá nhiều nơi và rất khó để biết hàm nào đã làm thay đổi biến đó. Các hàm không nên chia sẻ nhau giữa các biến, dữ liệu
```javascript
    const x = {
        val: 2
    };
    const x1 = () => x.val += 1;
    const x2 = () => x.val *= 2;
    x1();
    x2();
    console.log(x.val); // 6
```
    Cả 2 hàm x1 và x2 đều làm thay đổi giá trị thuộc tính val của x. Nếu trường hợp có rất nhiều hàm tác động đến thuộc tính val của x thì ta sẽ rất khó để kiểm soát nó, không biết được hàm nào làm nó ra như vậy.
    Nên chúng ta nên tranh chia sẻ biến (Avoid share state) để trách việc khó kiểm soát các hàm của mình
#### Avoid mutating state
    Tương tự như share state, nó sẽ làm ảnh hưởng đến biến được sử dụng trong chương trinh dẫn đến rất khó kiểm soát và debug
#### Side effects
    A side effect(hiệu ứng phụ) là tất cả những thay đổi của state trong ứng dụng mà nó nằm bên ngoài hàm đang thực thi. Side effect bao gồm:
        -Thay đổi biến bên ngoài và thuộc tính của đối tượng(global variable, parent scope chain) giống như share state và mutangting state
        -Console
        -Ghi lên file
