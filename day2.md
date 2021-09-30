# Bất đồng bộ và cách xử lí
## Bất đồng bộ
### Khái niệm
    Đây là quá trình mà các câu lệnh có thể chạy cùng một lúc chứ không cần chờ câu lệnh trước.
#### VD: Mình có một task nhiệm vụ bao gồm nấu cơm mất 10p,đánh răng 5p và quét nhà mất 5p. Nếu theo đồng bộ, ta sẽ phải thực hiện lần lược các nhiệm vụ nấu cơm 10p , sau đó mới đánh răng 5p và quét nhà 5p thì sẽ mất tổng cộng 20p. Nhưng nếu như theo bất đồng bộ, ta sẽ khởi động cùng 3 nhiệm vụ mà không chờ lần lượt xong nhiệm vụ này mới làm nhiệm vụ khác, có thể hiểu như là trong lúc đợi nấu cơm mình có thể cả đánh răng và rửa mặt và tổng thời gian sẽ mất có thể chỉ 10p.
### Ưu và nhược điểm
#### Ưu điểm: Giup chúng ta tối ưu được thời gian chạy của câu lệnh. Cũng giúp cho chúng ta thực hiện các câu lệnh tốn thời gian mà không làm ảnh hưởng đến luồng chính của chương trình
#### Nhược điểm: Thứ tự các câu lệnh được thực hiện đồng thời và kết quả trả về cũng không theo thứ tự nên đôi khi gây ra khó khăn khi kiểm soát câu lệnh và debug.
## Cách xử lí bất đồng bộ
### Callback
    Hiểu đơn giản là truyền một hàm B vào hàm A dưới dạng tham số, đến thời điểm thích hợp thì hàm A sẽ gọi hàm B để chạy
```javascript
    function asyncFunction(callback){
        console.log("Start");                 
        setTimeout(()=>{
            callback();                       /// hàm setTimeout này sẽ chờ 1s rồi mới gọi hàm callback();
        },1000);
        console.log("Waiting");
    }
    let printEnd= function(){
        console.log("End");
    }
    asyncFunction(printEnd);               
    /// Màn hình sẽ in lần lượt Start, Waiting, End do Start vs Waiting được gọi đồng thời theo thứ tự trên xuống mà k cần đợi Start vs setTimeout chạy xong.
```
#### Callback hell
    Khi quá nhiều câu lệnh bất đồng bộ, ta cần phải lồng từng callback đấy vào nhau, khiến cho code vô cùng khó đọc và debug, gọi là Callback Hell
```javascript
    function getData(link, callback) {
        setTimeout(() => {
        callback();
        }, 1000)
    }

    getData("Data1", () => {
    getData("Data2", () => {
        getData("Data2", () => {
            getData("Data3", () => {
                getData("Data4", () => {
                    getData("Data5", () => {
                        getData("Data6", () => {
                        console.log("Done");                     
                    })
                })
            })
         })
      })
   })
})
```
### Promise
    Có thể hiểu là lời hứa cho một giá trị chưa tồn tại và giá trị đó sẽ được trả về vào một thời gian trong tương lai
    Ví dụ bạn mua một món hàng mất 2 ngày mới về. Chủ shop đã thực hiện với bạn 1 lời hứa 2 ngày sau hàng sẽ về. Sau đó bạn vẫn ăn,ngủ, hoạt đồng bình thường và sau 2 ngày thì bạn sẽ nhận hàng
#### Cách dùng
```javascript
    let promise = new Promise ((resolve,reject)=>{
        /// Code
        resolve();  /// khi thành công
        reject();   /// khi thất bại
    })
    promise.then()
    .catch()
    .finally()
```
    Trong đó:
    - resolve: là một hàm callback xử lý cho hành động thành công
    - reject: là một hàm callback xử lý cho hành động thất bại
    Promise cung cấp cho ta 3 phương thức để xử lí dữ liệu sau khi thực hiện :
    - then(): dùng để xử lí sau khi Promise được thực hiện thành công (khi resolve được gọi)
    - catch(): dùng để xử lí sau khi Promise có bất kì một lỗi nào (khi reject được gọi)
    - finally(): khi promise thực hiện thành công hay thất bại thì nó cũng gọi đến finally.Nó được gọi xong khi chạy xong then/catch.
#### Ví dụ
```javascript
    var fs = require('fs');
    function readFilePromise(path){ 
        return new Promise((resolve,reject)=>{
            fs.readFile(path, {encoding: "utf8"},function(err,data){    /// hàm đọc file dữ liệu
                if(err){reject(err)}    /// gọi hàm reject khi bị lỗi
                else{
                    resolve(data)       /// gọi hàm resolve khi lấy được data
                }
            })
        })
    }
    readFilePromise('name.txt')
        .then(function(data){           /// bắt được dư liệu thì in ra
            console.log(data);
        })
        .catch(function(err){           /// bắt được lỗi bất kì thì in lỗi ra
            console.log(err);
        })
```
#### Trường hợp đặc biệt
##### Nối nhiều promise
```javascript
    function getData(url){
        // trả về 1 promise ở đây
        // gửi 1 request lấy dữ liệu
        // sau khi lấy về kết quả, xử lí promise với dữ liệu nhận được
    }
    var promise = getData('url');
    promise.then(function(result){  // có dữ liệu từ 'url' tại đây
        return getData(result); // trả về 1 promise khác
    }).then(function(result){
        //ở đây chưa kết quả promise vừa trả về ở trên và xử lí kết quả cuối cùng
    })
```
    Nếu bạn trả về 1 Promise ở hàm then() thứ nhất, thì hàm then() thứ hai sẽ chờ và chỉ được gọi cho đến khi Promise ở hàm then() thứ nhất thực hiện xong xuôi.
##### Promise có return và không return trong hàm then()
    Khi promise có nhiều hàm then() thì giá trị return của then đứng trước có thể làm tham số cho giá trị của then đứng sau. Còn then đứng trước không return thì tham số của then đứng sau sẽ là undefined
```javascript
    function getData(url){
        // trả về 1 promise ở đây
        // gửi 1 request lấy dữ liệu
        // sau khi lấy về kết quả, xử lí promise với dữ liệu nhận được
    }
    var promise = getData('url');
    promise.then(function(){
        console.log('nhan du lieu thanh cong');
        return 1;                   /// ở đây return về giá trị 1
    }).then(function(data){         /// tham số data ở đây sẽ nhận giá trị 1 từ return của then trước
        console.log(data)           ///  in ra giá trị 1
    }).then(function(data){         ///hàm then trước k return nên data ở đây sẽ là undefined
        console.log(data);          /// in ra undefined 
    }).catch((err)=>console.log(err));
```
### Async/Await
    Được xây dựng trên Promise và tương thích với tất cả Promise dựa trên API.
#### Cách dùng
    Async được dùng để khai báo một hàm bất đồng bộ. Các hàm bất đồng bộ sẽ luôn trả về một giá trị. Việc sử dụng async chỉ đơn giản là ngụ ý rằng một lời hứa sẽ được trả lại và nếu một lời hứa không được trả lại, JavaScript sẽ tự động kết thúc nó.
    Await được sử dụng để chờ một Promise. Nó chỉ có thể được sử dụng bên trong một khối Async. Từ khóa Await làm cho JavaScript đợi cho đến khi promise trả về kết quả. Cần lưu ý rằng nó chỉ làm cho khối chức năng không đồng bộ chờ đợi chứ không phải toàn bộ chương trình thực thi.
#### Ví dụ

```javascript
    async()=>{
        await this.setState({_id:item._id});                   /// đợi set giasd trị _id cho state đã
        await this.props.deleteBook(this.state._id);           /// gọi hàm deleteBook sử dựng input là _id của state
    }
```
    Khi có lỗi, thực hiện tương tự 1 promise
```javascript
    async function f() {

    try {
        let response = await fetch('http://no-such-url');
        } 
    catch(err) {
        alert(err); 
        }
    }

f();
```
## Ưu nhược điểm của callback,promise,async/await
### Callback
    Ưu điểm: Xử lí được vấn đề bất đồng bộ. Tuy nhiên chỉ nên dùng với trường hợp ít câu lệnh bất đồng bộ
    Nhược điểm: Khi quá nhiều câu lệnh bất đồng bộ, ta cần phải lồng từng callback đấy vào nhau, khiến cho code vô cùng khó đọc và debug, gọi là Callback Hell 
### Promise
    Ưu điểm: Xử lí được vấn đề bất đồng bộ. Có thể nối được nhiều Promise với nhau để xử lí bất đồng bộ lồng nhau, từ đó tránh được Callback hell
    Nhược điểm: Hơi khó chịu vì phải truyền callback vào hàm then và catch. Code cũng sẽ hơi dư thừa và khó debug.
### Async/Await
    Ưu điểm: Ra đời vào ES7,giải quyết được các vấn đề của Promise. Bản chất của async/await vẫn giống 1 Promise. Ngắn gọn,sạch sẽ. Dễ debug hơn. Dùng được câu lệnh điều kiện

 