# Higher Order Function
## Định nghĩa , đặc điểm
    Bình thường như mọi lúc, khi ta định nghĩa một Function, ta thường nhận param là các String, Number, Boolean, Array hoặc Object rồi hàm lại return về các String, Number, Boolean, Array hoặc Object.
    Đối với Higher-Order-Function , param có thể là Function, và lại return ra cũng có thể là Function
    Ví dụ:
```javascript
    var sum=function tong(a){
        return function (b) {
            return a+b;
        }
    }
```
    Hàm tong return về 1 function trả về giá trị a+b. Ở đây hàm tong là 1 HOF
    Trong Higher-order function có 3 khái niệm, đó là callback function, closure và currying
## Callback function, closure và currying
### Callback
    Là 1 hàm được truyền làm đối số cho một hàm khác và được thực thi khi hàm nhận đối số gọi lại 
    *xem ở bất đồng bộ*
### Closure
    Để tìm hiểu về closure, thì phải hiểu về lexical scoping. Lexical scoping là cách mà ta có thể dùng các biến được khai báo ở 
    function cha ngay tại function con, bởi vì function con chứa scope của function cha.
    Nếu function cha return về function con thì điều đó gọi là closure 
    Đặc điểm:
    - Là một hàm có thể ghi nhớ nơi nó được tạo ra và truy cập được biến ở ngoài phạm vi của nó
    - Có tính khép kín, tính bao đóng của javascript
```javascript
    function createVariable(){
        let variable=0;
        function upVariable(){
            return ++variable;
        }
        return upVariable;
    }
    var test=createVariable();
    console.log(createVariable()()); /// trả về 1
    console.log(createVariable()()); /// trả về 1
    console.log(test());         /// trả về giá trị 1
    console.log(test());         /// trả về giá trị 2
    console.log(test());         /// trả về giá trị 3
```
    Khi chạy hàm createVariable thì sẽ return về hàm upVariable, và chưa hề chạy qua đoạn code trong hàm upVariable.
    Trong trường hợp này test đã tham chiếu đến một instance upVariable
    Ta có thể gọi createVariable là một closure function
    Trong hầu hết ngôn ngữ lập trình, biến cục bộ bên trong hàm thực thi. Nhưng trong js, *test* tham chiếu đến instance upVariable nên giúp duy trì variable tồn tại .
    Vì vậy khi gọi hàm test, giá trị biến x vẫn được tính toán và được đưa vào hàm
### Currying 
    Là một kĩ thuật cho phép chuyển đổi một function nhiều tham số thành những function liên tiếp có một tham số.
#### VD1:
```javascript
    /// function nhiều tham số
    /// đây cũng là closure khi hàm cha trả về hàm con
    function Sum(a){
        return function(b){
            return function(c){
                console.log(a+b+c);
            }
        }
    }
    Sum(1)(2)(3) /// sẽ trả về tông của 1+2+3=6
    /// với kĩ thuật mới từ ES6: arror function
    var sum = a=>b=>c=>a+b+c;
    sum(1)(2)(3) /// trả vê giá trị 6
```
#### VD2: Khi giải 3 bài toán cùng lúc
```javascript
    /// Làm theo cách bình thường
    /// Tìm số lẻ bé hơn 10
    const findNumber1 = () => {
        const result = [];
        for (let i = 0; i < 10; i++) {
            if (i % 2 === 1) {
                result.push(i);
            }
        }
        return result;
    };
    /// Tim số chắn bé hơn 20
    const findNumber2 = () => {
        const result = [];
        for (let i = 0; i < 20; i++) {
            if (i % 2 === 0) {
                result.push(i);
            }
        }
        return result;
    };
    /// Tìm số chia cho 3 dư 2 và bé hơn 30
    const findNumber3 = () => {
        const result = [];
        for (let i = 0; i < 30; i++) {
            if (i % 3 === 2) {
                result.push(i);
            }
        }
        return result;
    };
    findNumber1();
    findNumber2();
    findNumber3();



    /// Thay vì làm như thế ta có thể dùng currying. Code sẽ ngắn gọn hơn rất nhiều 
    const findNumber = (num)=> (func) => {
        const result= [];
        for(let i=0;i<num;i++){
            if(func(i)){
                result.push(i);
            }
        }
        return result;
    }
    findNumber(10)((number)=>number %2 ==1);
    findNumber(20)((number)=>number %2 ==0);
    findNumber(30)((number)=>number %3 ==2);
```
## Các method của HOF của Array: find,map,filter,reduce,forEach
### Method find
    - Dùng để tìm kiếm phần tử trong mảng thỏa mãn điều kiện
    - Phương thức find () trả về giá trị của phần tử đầu tiên trong mảng được cung cấp thỏa mãn hàm kiểm tra được cung cấp. Nếu không có giá trị nào đáp ứng chức năng kiểm tra, thì trả về undefined.
```javascript
    const array1 = [5, 12, 8, 130, 44];
    const found = array1.find(element => element > 10);
    console.log(found); // trả về giá trị đầu tiên lơn hơn 10 là 12
```
### Method map
    - Phương thức map() gọi một hàm call back trên mọi phần tử của mảng và trả về một mảng mới chứa kết quả
    - Phương thức map() nhận hai đối số được đặt tên, đối số đầu tiên là bắt buộc, trong khi đối số thứ hai là tùy chọn.
```javascript
    const array1 = [1,5,6,7,8];
    const array2 = array1.map(x=>x*2);
    console.log(array2);  /// trả về mảng [2,10,12,14,16]
```
### Method reduce
    Reduce là một phương thức sẵn có được sử dụng để thực thi một hàm lên các phần tử của mảng (từ trái sang phải) với một biến tích lũy để thu về một giá trị duy nhất. Là một phương thức quan trọng hay sử dụng trong lập trình hàm.
```javascript
    const array1 = [1,5,6,7,8];
    const tong=(a,b)=>a+b;
    console.log(array1.reduce(tong));  // 0+1+5+6+7+8=27 
    console.log(array1.reduce(tong,5));// 5+1+5+6+7+8=32
```
### Method filter
    Phương thức filter () tạo một mảng mới với tất cả các phần tử vượt qua bộ lọc được thực hiện bởi hàm được cung cấp
```javascript
    const array1 = [1,5,6,7,8];
    const result = array1.filter(x=>x>5);
    console.log(result); /// trả vể mảng mới [6,7,8]
```
### Method forEach
    Phương thức forEach () thực thi một hàm được cung cấp một lần cho mỗi phần tử mảng
```javascript
    const array1 = [1,5,6,7,8];
    array1.forEach(x=>console.log(x*2)); //trả về 2  10  12   14  16    
```