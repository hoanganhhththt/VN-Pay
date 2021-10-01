# ES6
## Định nghĩa, cách dùng và so sánh
### Định nghĩa
    ES6 là chữ viết tắt của ECMAScript 6, đây được coi là một tập hợp các kỹ thuật nâng cao của Javascript và là phiên bản mới nhất của chuẩn ECMAScript.
### So sánh
    Bản chất của ES6 và các phiên bản khác thì vẫn là Javascript. Tuy nhiên với ES6 thì khác với ES5, ES6 cần biên dịch để chạy và ES6 có một số khái niệm mới. Nó giúp cho các lập trình viên code js một các tốt ưu và clear nhất
### Tính năng của ES6
    - Let & const
    - Template Literals
    - Arrow Function
    - Enhanced Object Literals
    - Default Parameter
    - Speard
    - Classes
    - Module
    - Promise
    - As

## Let and const
### Let
    Block - Scope là phạm vi trong một khối, nghĩa là chỉ hoạt động trong phạm vi hoạt động được khai báo bởi cặp {}
    Bình thường khi khai báo hay định nghĩa một biến trong hàm ta hay sử dụng var, nó vẫn đúng tuy nhiên chưa tối ưu.
    Ví dụ như mình muốn biến chỉ tồn tại trong câu lệnh if thôi không cần thiết ở bên ngoài nữa thì khai báo bằng var sẽ gây ra tốn bộ nhớ. 
    => Chính vì điều ấy, ở ES6 đã tối ưu lỗi ấy bằng khai báo 'let'. Let chỉ tồn tại trong phạm vi khối của nó (Block Scope)
```javascript
    var x = 10; // x ở đây cũng bằng 10
    {let x=2}; // x trong này bằng 2
    // x ở ngoài này bằng 10
```
### Const
    Const dùng để khai báo cho một hằng số (một biến JS với một giá trị không đổi)
    Const tương tự biến let, nhưng không thay đổi được giá trị
```javascript
     var x = 10; // x ở đây cũng bằng 10
    {const x=2}; // x trong này bằng 2
    // x ở ngoài này bằng 10
```
## Arrow function
    Arrow function cho phép viêt một cú pháp ngắn gọn để viết biểu thức hàm
    Bạn không cần function làm từ khóa, return từ khóa và dấu ngoặc nhọn
    Tuy nhiên hãy cẩn thận với từ khóa this trong Arrow function. Arrow function không phù hợp để xác định các phương thức đối tượng
    Bạn có thể bỏ qua return nếu hàm chỉ trả về 1 câu lệnh duy nhất.
```javascript
    //ES5
    function tong(x,y){
        return x+y
    };
    //ES6
    const tong = (x,y)=>x+y;   // bỏ qua return nếu hàm chỉ có 1 câu lệnh;
```
## Template Literals in ES6
    Là cách hiển thị các biến trong chuối theo kiểu mới
```javascript
    //ES5
    var first = 'Le';
    var last = 'Anh'
    var name = 'My name is: '+first+last+'.';
    console.log(name); /// My name is: Le Anh.
    //ES6
    var first = 'Le';
    var last = 'Anh'
    var name = `My name is: ${first} ${last}.`;
    console.log(name); /// My name is: Le Anh.
```
## Enhanced Object Literals in ES6
### Định nghĩa key: value cho Object
```javascript
    //thông thường khi tạo đối tượng
    var name = 'HoangAnh';
    var age = 23;
    var person = {
        name:name,
        age:age
    };
    console.log(person) // {name:'HoangAnh',age:23};
    //ES6
    var name = 'HoangAnh';
    var age = 23;
    var person = {name,age} // ES6 sẽ tự định nghĩa key:value cho Object
    console.log(person) // {name:'HoangAnh',age:23};
```
### Định nghĩa method cho object
```javascript
    //thông thường
    var name = 'HoangAnh';
    var age = 23;
    var person = {
        name:name,
        age:age,
        getName: function(){
            return name;
        }
    };
    console.log(person.getName()) // trả về HoangAnh
    //ES6
    var name = 'HoangAnh';
    var age = 23;
    var person = {
        name:name,
        age:age,
        getName(){
            return name;
        }
    };
    console.log(person.getName()) // trả về HoangAnh
```
### Định nghĩa key cho object dưới dạng biến
```javascript
    var first = 'name'
    var last = 'age'
    var person = {
        [first]:'HoangAnh',
        [last]:23
    }
    console.log(person);//{name:'HoangAnh',age:23}
```
## Default Parameters in ES6
```javascript
    //thông thường
    function logger(log){
        if(typeof log=== 'undefined'){
            log = 'Giá trị mặc định'
        }
        console.log(log);
    }
    logger()/// trả về giá trị mặc định
    // ES6
    function logger(log='Giá trị mặc định'){
        console.log(log);
    }
    logger()// trả về giá trị mặc định
    logger('haha') // trả về haha
```
## Speard
```javascript
    //VD1
    function logger({name,age,...rest}){
        console.log(name); // HoangAnh
        console.log(age);  // 23
        console.log(rest); // {sex:'Male',ponin:10}
    }
    logger({
        name:'HoangAnh',
        age:23,
        sex:'Male',
        point:10
    })
    //VD2
    function logger([a,b,...rest]){
        console.log(a); // 2
        console.log(b);  // 3
        console.log(rest); //[4,5,6,7]
    }
    logger([2,3,4,5,6,7])
    //VD3
    var array1 = ['FB','IG'];
    var array2 = ['Tik tok','Zalo','VNPay']
    var array3 = [...array1,...array2] /// nối 2 array1 và array2 vào array3
    console.log(array3); // ['FB', 'IG', 'Tik tok', 'Zalo', 'VNPay']
    //VD4
    const obj = {
        name:'HoangAnh',
        age:23,
        sex:'male'
    }
    var obj1 = {
        ...obj,    // copy các thuộc tính của obj
        name:'Le Hoang Anh' // sửa giá trị của key name
    }
    console.log(obj1);
```
## Classes
    Cách viết khác của constructor function
```javascript
    function Course(name,price){
        this.name = name;
        this.price = price;
        this.getName = ()=>this.name;
    }
    const phpCourse = new Course('PHP',1000);
    const jsCourse = new Course('JS',1200);
    console.log(phpCourse);
    console.log(jsCourse);
    //Class có constructor. Nó có thể định nghĩa được thuộc tính, phương thức như VD
    class Course{
        constructor(name,price){
            this.name= name;
            this.price = price;
        }
        getName() {
            return this.name;
        }
        getPrice(){
            return this.price
        }
    }
```
## Modules
### Export/Import
    Hiểu đơn giản là lấy dữ liệu được export từ file khác
```javascript
    /// ở file module.js
    var getName = ()=>'Lê Hoàng Anh';
    var domain = 'url.com';
    //export {getName,domain}
    export getName;
    export domain;

    /// ở file main.js
    import {getName,domain} from 'module.js'
    getName();   // 'Lê Hoàng Anh'
    console.log(domain); // 'url.com'
    /// hoặc cách khác
    import * as name from 'module.js'
    name.getName();
    console.log(name.domain);
```
### Export default
```javascript
    /// ở file module.js
    var getName = (name)=>name;
    export default getName;
    /// ở file main.js
    import name from 'module.js'
    name('HoangAnh'); /// HoangAnh
```
Tùy theo trường hợp export mà sẽ import theo cách khác nhau
## Promise
    Promise được đưa vào Javascript từ ES6, đây có thể coi là một kỹ thuật nâng cao giúp xử lý vấn đề bất đồng bộ hiệu quả hơn. Giúp tránh được trường hợp Callback hell
### Cách dùng
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
### Ví dụ
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
### Trường hợp đặc biệt
#### Nối nhiều promise
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
#### Promise có return và không return trong hàm then()
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
## As
    Phép gán hủy cấu trúc mảng và đối tượng
### Phép gán hủy cấu trúc mảng
```javascript
    let colors = ["Xanh", "Đỏ"];
    let [a, b] = colors; // Phép gán hủy cấu trúc mảng
    console.log(a); // Xanh
    console.log(b); // Đỏ
    // trường hợp mảng nhiều phần tử hơn
    let colors = ["Xanh", "Đỏ", "Tím"];
    let [a, ...r] = colors;
    console.log(a); // Xanh
    console.log(r); // ["Đỏ", "Tím"]
```
### Phép gán hủy cấu trúc đối tượng
```javascript
    let school = { name: "NIIT", age: 18 };
    let { name, age } = school; // Phép gán hủy cấu trúc đối tượng
    console.log(name); // NIIT
    console.log(age); // 18
```


