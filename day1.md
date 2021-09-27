# Array
## Khái niệm, cách tạo, kiểm tra dữ liệu, truy xuất Array
### Khái niệm: 
    Mảng là một biến đặc biệt, có thể chứa nhiều giá trị cùng 1 lúc. 
    VD:List các món ăn, Tổng hợp điểm số, Tủ đựng sách chứa nhiều sách...
### Cách tạo: 
    Có thể dùng const, var, let để tạo mảng đều được. Tùy vào nhu cầu sử dụng của người tạo. Tạo mảng trong js có thể không cần khai báo số lượng phần tử có trong mảng và nó có thể chứa nhiều kiểu biến cùng lúc

    ```javascript
        const newArr = [1,'a','u',''];
        var myArray = ["VNPAY","Techcombank","Viettinbank"];   
        let arr=[];
        arr[0]=1;
        const books = new Array("toán","văn","ngoại ngữ");
    ```
### Kiểm tra dữ liệu:
    Có thể kiếm tra xem nó có phải là một mảng không bằng phương thức "Array.isArray(array)" nó sẽ trả về true nếu đó là mảng và false nếu đó không phải là mảng 

        ```javascript
        let arr=[];
        let arr1={};
        Array.isArray(arr);/// trả về true
        Array.isArray(arr1);/// trả về false
        ```
### Truy xuất Array:
    - Ta có thể truy cập một phần tử của mảng bằng cách tham chiếu đến số chỉ mục
    - Số chỉ mục bắt đầu từ 0. 0 là phần tử đầu tiên của mảng

    ```javascript
        let arr=[1,2,3,4,5]
        console.log(arr[0]); /// trả về giá trị đầu tiên là 1
        console.log(arr[1]); /// trả về giá trị thứ 2 là 2
  ```
## Các method làm việc với Array
### Chuyển đổi mảng thành chuối bằng method toString hoặc join
    - Method toString: Chuyển một mảng thành chuỗi các giá trị mảng (được phân tách bằng dấu phẩy)
        

    ```javascript
        var foods = ['rice','chicken','pizza'];
        console.log(fooods.toString());   /// trả về rice,chicken,pizza    
    ```
    - Method join: Tương tự toString nhưng có thể chỉ định dấu phân tách

    ```javascript
        var foods = ['rice','chicken','pizza'];
        console.log(foods.join('-'))      /// trả về rice-chicken-pizza
    ```

### Thêm/tách phần tử cuối mảng bằng method push và pop
    - Method push: Thêm phần tử vào cuối mảng
    - Method pop: Tách phần tử cuối mảng ra khỏi mảng

    ```javascript
        var foods = ['rice','chicken','pizza']
        foods.push('noodle'); 
        console.log(foods);   /// trả về mảng ['rice','chicken','pizza','noodle']. phần tử 'noodle' được thêm vào cuối mảng
        var x =  foods.pop();
        console.log(x);       /// trả vê giá trị 'noodle' vì 'noodle' ở vị trí cuối của mảng
        console.log(foods);   /// trả về mảng foods mới không có giá trị 'noodle'. ['rice','chicken','pizza']
    ```
### Thêm/tách phần tử đầu mảng bằng method shift và unshift
    - Method shift: Loại bỏ phần tử đầu tiên ra khỏi mảng và thay đổi số thứ tự của mảng đó
    - Method unshift : Thêm giá trị mới vào vị trí đầu tiên của mảng và thay đổi số thứ tự của mảng đó

    ```javascript
        var foods = ['rice','chicken','pizza']
        var x = foods.shift();
        console.log(x);      /// trả về giá trị rice
        console.log(foods);  /// lúc này foods đã mất giá trị rice chỉ còn lại 2 phần tử là ['chicken','pizza']
        foods.unshift(x);
        console.log(foods);  /// lúc này giá trị x đã được thêm vào đầu mảng foods. lúc này foods là ['rice','chicken','pizza'] 
    ```
### Thay đổi giá trị của phần tử bất kì trong mảng
    - Ta sử dụng số chỉ mục trong mảng và gán lại giá trị mới cho phần tử đó thì mảng lúc này sẽ thành mảng mới có giá trị mới ở số chỉ mục đó

    ```javascript
        var foods = ['rice','chicken','pizza']
        foods[0]='noodle';
        console.log(foods); /// ['noodle','chicken','pizza']
    ```
### Xóa phần tử trong mảng bằng method delete
    Xóa phần tử theo số chỉ mục và giá trị tại số chỉ mục đó sẽ trở thành undefined

    ```javascript
        var foods = ['rice','chicken','pizza']
        console.log(foods[1]); /// trả về giá trị 'chicken'
        delete foods[1];
        console.log(foods[1]); /// trả về giá trị undefined
        console.log(foods);    /// ['rice',undefined,'pizza']
    ```
### Nối/thay thế phần tử mảng bằng method splice
    Method splice có thể nhận 3 tham số input. 
    Trong đó:
        - input thứ nhất là số chỉ mục vị trí bắt đầu để thay thế hoặc nối thêm phân tử vào mảng 
        - input thứ 2 xác đinh có bao nhiêu phần tử được thay thế tính từ vị trị bắt đầu. Nếu để là 0 thì dùng để nối phần tử vào mảng
        - phần còn lại dùng để xác định các phần tử mới được thêm vào/thay thế

    ```javascript
        var foods = ['rice','chicken','pizza']
        var x='noodle';
        foods.splice(1,0,x);
        console.log(foods);    /// ['rice','noodle','chicken','pizza']
        var removed = foods.splice(1,1);
        console.log(removed);  /// 'noodle'
        console.log(foods);    /// ['rice','chicken','pizza']
    ```
### Hợp nhất(nối) nhiều mảng bằng method concat
    Tạo ra mảng mới bằng cách sát nhập mảng hiện có

    ```javascript
        var foods = ['rice','chicken','pizza'];
        var drinks = ['water','tea','milk'];
        var eats = foods.concat(drinks);
        console.log(eats);  ///['rice','chicken','pizza','water','tea','milk']
        console.log(foods); /// vẫn giữ nguyên giá trị ['rice','chicken','pizza']
    ```
### Cắt mảng bằng method slice
    Tách 1 phần của mảng ra thành mảng mới mà không thay đổi mảng cũ
    Cách dùng: Array.slice(số chỉ mục bắt đầu tách,số chỉ mục kết thúc tách *để không thì sẽ tách hết mảng*)

    ```javascript
        var foods = ['rice','chicken','pizza'];
        var newArr = foods.slice(1);
        var newArr1 = foods.slice(0,2);
        console.log(foods);    ///['rice','chicken','pizza'];
        console.log(newArr);   ///['chicken','pizza'];
        console.log(newArr1);  ///['rice','chicken'];
    ```
### Sắp xếp mảng bằng method sort,reverse
#### Phân loại mảng theo thứ tự abc
    ```javascript
        const fruits = ["Banana", "Orange", "Apple", "Mango"];
        console.log(fruits.sort())  ///['Apple', 'Banana', 'Mango', 'Orange']
    ```
#### Đảo ngược mảng
    Đảo vị trí các phần tử trong mảng, đầu chuyển về cuối,cuối lên đầu

    ```javascript
        const fruits = ["Banana", "Orange", "Apple", "Mango"];
        console.log(fruits.reverse())// ['Mango','Apple','Orange','Banana']
    ```
#### Sắp xếp theo số 
    Hàm sort sẽ sắp xếp các giá trị dưới dạng chuỗi nên nếu dùng sort không nó sẽ xếp 25 lớn hơn 100 vì 2 lớn hơn 1. Ta phải cung cấp cho nó 1 hàm so sánh

    ```javascript
        const numbers=[10,20,35,100,45,40];
        console.log(numbers.sort());
        numbers.sort((a,b)=>a-b);            ///[10, 100, 20, 35, 40, 45]
        console.log(numbers);                ///[10, 20, 35, 40, 45, 100]
    ```     
### Tìm max,min của mảng bằng Math.max/Math.min
    Tìm giá trị lớn nhất và nhỏ nhất của mảng nhưng tham số đầu vào của nó phải là số nên ta phải sử dụng *...* để copy số trong mảng ra

    ```javascript
        const numbers=[10,20,35,100,45,40];
        console.log(Math.max(...numbers));  /// trả về 10
        console.log(Math.min(...numbers));  /// trả về 100
    ```
## So sánh các method trong Array
### Chuyển mảng thành chuỗi:toString và join
    Cả 2 method này đều thực hiện chức năng chuyển mảng thành chuỗi. Tuy nhiên nếu dùng toString thì nó sẽ tạo ra chuối mới với các phần tử trong mảng nối tiếp nhau ngăn cách bằng dầu ",". Còn nếu dùng join ta có thể lựa chon bất kì giá trị nào ngăn cách giữa các phần tử sau khi tạo thành chuỗi

    ```javascript
        var foods = ['rice','chicken','pizza'];
        console.log(fooods.toString());   /// trả về rice,chicken,pizza    
    ```
    ```javascript
        var foods = ['rice','chicken','pizza'];
        console.log(foods.join('-'))      /// trả về rice-chicken-pizza
        console.log(foods.join(' '))      /// trả về rice chicken pizza
    ```
### Thêm phần tử vảo mảng 
    Có 3 cách thêm phần tử vào mảng đó là unshift,push,splice
#### unshift sẽ thêm vào đầu mảng
    
```javascript
        var foods = ['rice','chicken','pizza']
        var x = 'noodle'
        foods.unshift(x);
        console.log(foods);  ///['noodle', 'rice', 'chicken', 'pizza']
```
#### push sẽ thêm vào cuối mảng

    ```javascript
        var foods = ['rice','chicken','pizza']
        foods.push('noodle'); 
        console.log(foods);   /// trả về mảng ['rice','chicken','pizza','noodle']
    ```
#### splice sẽ thêm vào vị trí bất kì trong mảng
    ```javascript
        var foods = ['rice','chicken','pizza']
        foods.splice(1,0,'ice scream');
        console.log(foods);   ///['rice', 'ice scream', 'chicken', 'pizza']
    ```

### Xóa phần tử khỏi mảng
    Có 4 cách xóa phần tử khỏi mảng là pop,shift,splice,delete
#### Xóa phần tử cuối cùng
    ```javascript
        var foods= ['rice','chicken','pizza'];
        foods.pop();
        console.log(foods); /// trả về ['rice','chicken']
    ```
#### Xóa phần tử đầu tiên
    ```javascript
        var foods= ['rice','chicken','pizza'];
        foods.shift();
        console.log(foods); /// trả về ['chicken','pizza']
    ```
#### Xóa phần tử theo số chỉ mục tuy nhiên để lại giá trị undefined
    ```javascript
        var foods= ['rice','chicken','pizza'];
        delete foods[1];
        console.log(foods); ///trả về ['rice', empty, 'pizza']
    ```
#### Xóa được nhiều phần tử cùng lúc ở vị trí bất kì
    ```javascript
        var foods= ['rice','chicken','pizza'];
        foods.splice(1,1);
        console.log(foods); /// xóa 1 phần tử từ số chỉ mục 1. Trả về ['rice','pizza']
    ```
## Copy array và những cách hay sử dụng? Ưu nhược điểm và performance mỗi cách
### Cách không cần duyệt từng phần tử mảng
#### Sử dụng method slice
    ```javascript
        const numbers = [1, 2, 3, 4, 5];
        const copy = numbers.slice();
        console.log(copy);    /// Array [1, 2, 3, 4, 5]
        console.log(numbers); /// Array [1, 2, 3, 4, 5]
    ```
##### Ưu nhược điểm và performance
    Ưu điểm: Code ngắn và gọn. Mảng cũ không bị thay đổi. Không cần duyệt từng phần tử của mảng
    Nhược điểm: 
    Performance: Nên sử dụng vì nó k cần duyệt từng phần tử mảng nên sẽ tốt hơn những cách dùng for,forEach,map,reduce

#### Sử dụng spread operator
    ```javascript
        const numbers = [1, 2, 3, 4, 5];
        const copy = [...numbers];
        console.log(copy);    ///Array [1, 2, 3, 4, 5]
        console.log(numbers); ///Array [1, 2, 3, 4, 5]
    ```
##### Ưu nhược điểm và performance
    Ưu điểm: Code ngắn và gọn. Mảng cũ không bị thay đổi. Không cần duyệt từng phần tử của mảng
    Nhược điểm: 
    Performance: Nên sử dụng vì nó k cần duyệt từng phần tử mảng nên sẽ tốt hơn những cách dùng for,forEach,map,reduce

#### Sử dụng method concat
    ```javascript
        const numbers = [1, 2, 3, 4, 5];
        const copy = numbers.concat();
        console.log(copy);    /// Array [1, 2, 3, 4, 5]
        console.log(numbers); /// Array [1, 2, 3, 4, 5]
    ```
##### Ưu nhược điểm và performance
    Ưu điểm: Code ngắn và gọn. Mảng cũ không bị thay đổi. Không cần duyệt từng phần tử của mảng
    Nhược điểm: 
    Performance: Nên sử dụng vì nó k cần duyệt từng phần tử mảng nên sẽ tốt hơn những cách dùng for,forEach,map,reduce
### Cách sử dụng vòng lặp
#### Sử dụng method map
    ```javascript
        const numbers = [1, 2, 3, 4, 5];
        const copy = numbers.map(function(number) {
            return number;
        });
        console.log(copy);    /// Array [1, 2, 3, 4, 5]
        console.log(numbers); /// Array [1, 2, 3, 4, 5]
    ```
##### Ưu nhược điểm và performance
    Ưu điểm:  Mảng cũ không bị thay đổi. Code dễ hiểu
    Nhược điểm: Code dài,mất thời gian và phải lặp qua từng phần tử
    Performance: Sẽ k tốt bằng các cách không cần duyệt phần tử như slice,concat,spread operator
#### Sử dụng method forEach kết hợp push
    ```javascript
        const numbers = [1, 2, 3, 4, 5];
        let copy = [];
        numbers.forEach(function(value) {
            copy.push(value);
        });
        console.log(copy);    /// Array [1, 2, 3, 4, 5]
        console.log(numbers); /// Array [1, 2, 3, 4, 5]
    ```
##### Ưu nhược điểm và performance
    Ưu điểm:  Mảng cũ không bị thay đổi. Code dễ hiểu
    Nhược điểm: Code dài,mất thời gian và phải lặp qua từng phần tử
    Performance: Sẽ k tốt bằng các cách không cần duyệt phần tử như slice,concat,spread operator
#### Sử dụng vòng lặp for truyền thống
    ```javascript
        const numbers = [1, 2, 3, 4, 5];
        let copy = [];
        for (let i = 0; i < numbers.length; i++) {
            copy.push(numbers[i]);
        }
        console.log(copy);    /// Array [1, 2, 3, 4, 5]
        console.log(numbers); /// Array [1, 2, 3, 4, 5]
    ```
##### Ưu nhược điểm và performance
    Ưu điểm:  Mảng cũ không bị thay đổi. Code dễ hiểu
    Nhược điểm: Code dài,mất thời gian và phải lặp qua từng phần tử
    Performance: Sẽ k tốt bằng các cách không cần duyệt phần tử như slice,concat,spread operator
#### Sử dụng method reduce
    ```javascript
        const numbers = [1, 2, 3, 4, 5];
        const copy = numbers.reduce((arr, item) => {
            arr.push(item);
            return arr;
        }, []);
        console.log(copy);    /// Array [1, 2, 3, 4, 5]
        console.log(numbers); /// Array [1, 2, 3, 4, 5]
    ```
##### Ưu nhược điểm và performance
    Ưu điểm:  Mảng cũ không bị thay đổi. Code dễ hiểu
    Nhược điểm: Code dài và dùng nhiều hàm, mất thời gian và phải lặp qua từng phần tử
    Performance: Sẽ k tốt bằng các cách không cần duyệt phần tử như slice,concat,spread operator
#### Sử dụng phương thức apply
    ```javascript
        const numbers = [1, 2, 3, 4, 5];
        let copy = [];
        Array.prototype.push.apply(copy, numbers);
        console.log(copy);    /// Array [1, 2, 3, 4, 5]
        console.log(numbers); /// Array [1, 2, 3, 4, 5] 
    ```
##### Ưu nhược điểm và performance
    Ưu điểm:  Mảng cũ không bị thay đổi. Code ngắn
    Nhược điểm: Code dùng nhiều hàm ,mất thời gian và phải lặp qua từng phần tử
    Performance: Sẽ k tốt bằng các cách không cần duyệt phần tử như slice,concat,spread operator
#### Sử dụng Array.from()
    *Set* trong js là tập hợp các giá trị không bị trùng lặp, nghĩa là set không thể có 2 giá trị trùng nhau

    ```javascript
        const numbers = [1, 2, 3, 4, 5, 5];
        const copy = Array.from(new Set(numbers));
        console.log(copy);    /// Array [1, 2, 3, 4, 5, 5]
        console.log(numbers); /// Array [1, 2, 3, 4, 5]
    ```
##### Ưu nhược điểm và performance
    Ưu điểm:  Mảng cũ không bị thay đổi. Code ngắn
    Nhược điểm: DÙng nhiều hàm, mất thời gian và phải lặp qua từng phần tử. Loại bỏ phần tử bị trùng sẽ vừa là ưu điểm vừa là nhược điểm của nó tùy vào nhu cầu người code
    Performance: Sẽ k tốt bằng các cách không cần duyệt phần tử như slice,concat,spread operator
