# Kiến thức nền tảng React
## Main Concepts của ReactJS
### Hello world
    Bất cứ khi tìm hiểu về ngôn ngữ lập trình nào thì đầu tiên ta cũng sẽ học về cách in ra màn hình Hello World.
    React là một thư viện của Javascript nên sẽ k ngạc nhiên khi code react sử dụng javascript rất nhiều.
```javascript
    ReactDOM.render(
        <h1>Hello, world!</h1>,              // nó sẽ hiển thị "Hello, world!" trên trang
        document.getElementById('root')
    );
```
### Introducing JSX
    JSX rất giống với thẻ html tuy nhiên nó không phải là thẻ html
```javascript
    const element = <h1>Hello, world!</h1>;
```
    Nó được gọi là JSX, là một phần cú pháp mở rộng của Javascript. Nó được sử dụng trong Reactjs để làm giao diện người dùng
    JSX giúp React xử lý sự kiện, trạng thái thay đổi theo thời gian và cách dữ liệu được chuẩn bị để hiển thị
#### Nhúng JSX
    ```javascript
        function fullName(firstName,lastName){
            return firstName+' '+ lastName;
        }
        const user= {
            firstName:'Lê',
            LastName:'Hoàng Anh'
        }
        const element = (
            <h1>Hello, {fullName(user.firstName,user.lastName)}</h1> //có thể đặt bất kì biểu thức JS nào vào trong JSX băng {}
        )
        ReactDOM.render(
            element,                       
            document.getElementById('root')
        );
    ```
#### JSX is an Expression Too
    ```javascript
        function helloUser(user){
            if(user){
                return <h1>Hello, {user}!</h1>
            }else{
                return <h1> Nothing to say </h1>
            }
        }
    ```
#### Thuộc tính trong JSX 
    ```javascript
        const element = <h1 style={{color:'red'}}>Hello</h1>
        const element = <div tabIndex="0"></div>;
    ```
#### Thẻ con trong JSX
    Với một số thẻ bạn có thể đóng bằng <img />
```javascript
    const element = <img src={user.avatarUrl} />;
```
    Thẻ JSX có thể chứa các thẻ con như HTML
```javascript
    const element = (
        <div>
            <h1>Hello</h1>
            <h2>World</h2>
        </div>
    )
```
#### Tạo JSX với kiểu Object 
```javascript
    //cách thông thường
    const element = (
        <h1 className='greeting'>Hello</h1>
    )
    // Cách 2 dùng React.createElement()
    const element = React.createElement(
        'h1',
        {className:'greeting'},
        'Hello,world'
    );
```
### Rendering Elements
#### Hiển thị phần tử element vào DOM
    Trong React, để hiển thị element thành nút DOM gốc, sử dụng ReactDOM.render():
```javascript
    const element = <h1>Hello</h1>
    ReactDOM.render(element,document.getElementById('roor'));
```
#### Cập nhật phần tử JSX
```javascript
    function tick(){
        const element = (
            <div>
                <h1>Hello, world!</h1>
                <h2>It is {new Date().toLocaleTimeString()}</h2>
            </div>
        );
        ReactDOM.render(element, document.getElementById('root'));
    }
    setInterval(tick,1000) // hàm này sẽ gọi hàm tick sau mỗi 1s
```
    Như vậy cứ sau 1s, giao diên sẽ dk cập nhật lại 1 lần theo thời gian
### Components and Props
    Có 2 kiểu Components là Class Component và Function Component
```javascript
    // Class Component  
    class Welcome extends React.Component {
        render(){
            return <h1>Hello, {this.props.name} </h1>
        }
    }
    // Function Component 
    function Welcome (props){
        return <h1>Hello, {props.name} </h1>;
    }
```
#### Rendering a Component
```javascript
    function HelloUser(props){
        return <h1>Hello, {props.name}</h1>
    }
    const element = <HelloUser name='Hoàng Anh' />  // React sẽ call HelloUser componenet cùng với {name:'Hoàng Anh'} và cái này sẽ chính là props. Và đây cũng chính là cách truyền giá trị props từ component cha sang component con
    ReactDOM.render(
        element,            // in ra Hello, Hoàng Anh
        document.getElementById('root')
    );
```
#### Composing Components
    Bạn có thể tái sử dụng các component
```javascript
    function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {    /// Chúng ta tạo ra App component render ra nhiều Welcome component
  return (
    <div>
      <Welcome name="Hoang Anh" />
      <Welcome name="Hahaa" />
      <Welcome name="Hale" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```
#### Extracting Components
```javascript
    // Ví dụ:
    function Comment(props) {
        return (
            <div className="Comment">
                <div className="UserInfo">
                    <img className="Avatar"
                        src={props.author.avatarUrl}
                        alt={props.author.name}
                    />
                    <div className="UserInfo-name">
                        {props.author.name}
                    </div>
                </div>
                <div className="Comment-text">
                    {props.text}
                </div>
                <div className="Comment-date">
                    {formatDate(props.date)}
                </div>
            </div>
        );
    }
    // Ta có thể tạo 1 component Avatar riêng.Trong Avatar ta sẽ truyền cho nó thuộc tính user={props.auther} thì props của Avatar khi này sẽ có giá trị props.user tương đương vs props.auther của Comment component .Rồi truyền vào Comment Component
    function Avatar(props){
        return(
            <img className="Avatar"
                src={props.user.avatarUrl}
                alt={props.user.name}
            />
        )
    }
    function Comment(props) {
        return (
            <div className="Comment">
                <div className="UserInfo">
                    <Avatar user={props.author} />  
                    <div className="UserInfo-name">
                        {props.author.name}
                    </div>
                </div>
                <div className="Comment-text">
                    {props.text}
                </div>
                <div className="Comment-date">
                    {formatDate(props.date)}
                </div>
            </div>
        );
    };
    /// Hoặc có thể tạo UserInfo component 
    function UserInfo(props) {
        return (
            <div className="UserInfo">
                <Avatar user={props.user} />
                <div className="UserInfo-name">
                    {props.user.name}
                </div>
            </div>
        );
    };
    //Khi này Comment Component chỉ còn
    function Comment(props) {
        return (
            <div className="Comment">
                <UserInfo user={props.author} />
                <div className="Comment-text">
                    {props.text}
                </div>
                <div className="Comment-date">
                    {formatDate(props.date)}
                </div>
            </div>
        );
    }
```
#### Props are Read-Only
    Trong React, Props giống như một hằng, giá trị của nó ko thể thay đổi. Muốn thay đổi giá trị của nó thì có componenet cha mới thay đổi được props của component con. Khi sử dụng props ta chỉ nên đọc giá trị chứ không nên thay đổi giá trị của nó.
### State and Lifecycle
#### State
    - Giống như một biến được sử dụng trong Reactjs
    - State được tích hợp sẵn trong Reactjs
    - State là nơi lưu trữ các giá trị thuộc tính trong thành phần
    - Khi state thay đổi, component sẽ hiển thị lại
    - Mỗi component có một state riêng đối với Class Component.
    - State có thể có nhiều thuộc tính
```javascript
    class Clock extends React.Component {
        constructor(props) {
            super(props);
            this.state = {   //// Khai báo state, thuộc tính của state ở đây
                date: new Date(),
                name:'Hoàng Anh'    /// state là 1 đối tượng, có thể có nhiều thuộc tính
            };  
        }
        /// sử dụng this.state để sử dụng state của component này
        render() {
            return (
                <div>
                    <h1>Hello, world!</h1>
                    <h2>It is {this.state.date.toLocaleTimeString()}.</h2>   
                </div>
            );
        }
    }

    ReactDOM.render(
        <Clock />,
        document.getElementById('root')
        );
```
    Cách set giá trị cho state: dùng setState():
```javascript
    // thông thường
    this.setState({comment: 'Hello'});
    // với trường hợp có cả state và props để tránh bị đồng bộ ta nên sử dụng cách này
    this.setState((state, props) => ({
        counter: state.counter + props.increment
    }));
```
    Cách truyền dữ liệu qua các Component:
```javascript
    <FormattedDate date={this.state.date} />
    function FormattedDate(props) {
        return <h2>It is {props.date.toLocaleTimeString()}.</h2>;  //props.date ở đây sẽ tương đương vs this.state.date của component khác
    }
```
#### Lifecycle
    componentDidMount: method này được thực thi khi 1 component được render lên máy khách.
    componentWillUnmount: được gọi khi chúng ta unmout 1 component kiểu như xóa nó khỏi react.
```javascript
    class Clock extends React.Component {
        constructor(props) {
            super(props);
            this.state = {date: new Date()};
        }

        componentDidMount() {           
            this.timerID = setInterval(
                () => this.tick(),
                1000
            );
        }

        componentWillUnmount() {
            clearInterval(this.timerID);
        }

        tick() {
            this.setState({
            date: new Date()
            });
        }

        render() {
            return (
                <div>
                    <h1>Hello, world!</h1>
                    <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
                </div>
            );
        }
    }

    ReactDOM.render(
        <Clock />,
        document.getElementById('root')
    );
```
    Ngay sau khi component được render lên máy khách.
    Nó sẽ khởi tạo this.state với đối tượng bao gồm thuộc tính date có giá trị là thời gian hiện tại. 
    Sau đó React gọi đến phương thức render để cập nhật vào DOM ứng vs giá trị đồng hồ hiện tại.
    Sau khi giá trị thời gian hiện tại được in ra màn hình, React mới gọi đến phương thức componentDidmount
    Và hàm tick sẽ được gọi. Mỗi giây, trình duyệt gọi phương thức tick() một lần và như thế cứ 1s, nó sẽ cập nhập giá trị state 1 lần và render lại màn hình 1 lần.

    Khi component Clock bị xóa khỏi DOM, React sẽ goi phương thức componentWillUnmount() để bộ đếm thời gian dừng lại.
### Handling Events
    Gọi là xử lí sự kiện, giống như bên HTML thì react cũng có những hàm xử lí sự kiện cơ bản như : OnClick, onSubmit, onChange,...
```javascript
    class Toggle extends React.Component {
        constructor(props) {
            super(props);
            this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
        this.handleClick = this.handleClick.bind(this);
        }

        handleClick() {
            this.setState(prevState => ({
            isToggleOn: !prevState.isToggleOn
            }));
        }

        render() {
            return (
                <button onClick={this.handleClick}>
                    {this.state.isToggleOn ? 'ON' : 'OFF'}
                </button>
                <button onClick={()=>console.log('Ví dụ 2')}><button>
            );
        }
    }

    ReactDOM.render(
        <Toggle />,
        document.getElementById('root')
    );
```
### Conditional Rendering
    Ta sử dụng JSX để render trong React. JSX cho phép chúng ta sử dụng Pure JavaScript với các câu lệnh điều kiện quen thuộc
#### If else
```javascript
    function CheckUsers(users){
        if(users.length == 0){
            return <h2>No one</h2>
        }else{
            return (
                <div>
                    {users.map(user => <ListUsers key={user.id} user={user} />)}
                </div>
            );
        }
    }
```
#### Ternary Operation
    Cách dùng: condition ? trueExpression : falseExpression 
```javascript
    function TodoItem({item,isEditing}){
        return(
            <div>
                {isEditing
                    ?<TodoEdit item={item} />
                    :<TodoItem item={item} />
                }
            </div>
        )
    }
```
#### logical && operator
    Cách dùng: expression01 && expression02
    Được sử dụng khi muốn render component hoặc return null
    Nếu expression1 là true thì trả về expression2,nếu expression là false thì trả về null
```javascript
    // VD1:
    const result = true && 'Hello World';  
    console.log(result);
    // Hello World

    const result = false && 'Hello World';
    console.log(result);
    // false
```
#### Switch Case operator
```javascript
    const comment = ({text,state})=>{
        switch(state){
            case 'success':
                return <Success text={text} />;
            case 'error':
                return <Error text={text} />;
            case 'info':
                return <Info text={text} />;
            default:
                return null;
        }
    }
```
### Lists and Keys
    Thường sử dụng cho cho một mảng có chưa dữ liệu từ BackEnd gửi lên. Khi đó ta sẽ sử dụng map() để duyệt từng phần tử của mảng.
    Còn Keys sử dụng giúp kiểm soát từng dữ liệu trong mảng, thường thì id sẽ được sử dụng làm keys.
```javascript
    const liseItem = this.props.items.map((item,key)=>{
        return (
            <ul key={key}>
                {item.id}
                <li>{item.name}</li>
                <li>{item.age}</li>
            </ul>
        )
    })
```
### Forms
    Là biểu mẫu cho phép người dùng tương tác với trang web.
#### The textarea Tag
    Trong JSX, textarea khác HTML ở chỗ giá trị của nó được đặt trong một thuộc tính giá trị.
```javascript
    // Ví dụ
    <textarea value={this.state.text}></textarea>
    // Áp dụng
    import { useState } from "react";
    import ReactDOM from "react-dom";

    function MyForm() {
        const [textarea, setTextarea] = useState(
            "The content of a textarea goes in the value attribute"
        );

    const handleChange = (event) => {
        setTextarea(event.target.value)
    }

    return (
        <form>
            <textarea value={textarea} onChange={handleChange} />
        </form>
        )
    }

    ReactDOM.render(<MyForm />, document.getElementById('root'));
```
#### The select Tag
    Trong React, giá trị đã chọn được xác định bằng value thuộc tính trên select thẻ
```javascript
    function MyForm() {
        const [myCar, setMyCar] = useState("Volvo"); // khởi đầu sẽ là giá Volvo.

        const handleChange = (event) => {
            setMyCar(event.target.value)
        }

        return (
            <form>
                <select value={myCar} onChange={handleChange}>
                    <option value="Ford">Ford</option>
                    <option value="Volvo">Volvo</option>
                    <option value="Fiat">Fiat</option>
                </select>
            </form>
        )
    }
```
#### The file input tag
    Ta có thể tải lên một hoặc nhiều tệp từ bộ nhớ 
```javascript
    <input type='file' />
```
### Lifting State Up
    Hiểu đơn gian thì đây là kĩ thuật truyền các giá trị, dữ liệu từ component cha sang component con.
    Bằng cách truyền qua props của component con và thuộc tính của component cha.
```javascript
    import {Component} from React.Component
    const VNDtoUSD = function (props){
        const result = function(vnd){
            return vnd/23632;
        };
        return (
            <div>
                <span>VND</span>
                <input
                    onChange={(e)=>{
                        const vnd = e.target.value;
                        const usd = result(vnd);
                        props.onHandleChange({
                            vnd,
                            usd,
                        });
                    }}
                    value = {props.value} 
                />
            </div>
        );
    };
    const USDtoVND = function (props){
        const result = function(usd){
            return usd*23632;
        };
        return (
            <div>
                <span>USD</span>
                <input
                    onChange={(e)=>{
                        const usd = e.target.value;
                        const vnd = result(vnd);
                        props.onHandleChange({
                            usd,
                            vnd,
                        });
                    }}
                    value = {props.value} 
                />
            </div>
        );
    };
    export default class App extends Component{
        constructor(props){
            super(props);
            this.state={
                usd:0,
                vnd:0
            }
        };
        handleChange = (data)=>{
            this.setState(data);
        }
        render(){
            return(
                <div>
                    <USDtoVND onHandleChange={this.handleChange} value={this.state.usd} />
                    <VNDtoUSD onHandleChange={this.handleChange} value={this.state.vnd} />
                </div>
            )
        }
    }
```
    Khi mình nhập giá trị váo một trong 2 ô input VND hoặc USD thì nó sẽ trả về số tiền tương ứng ở ô input còn lại.
    2 conponent con USDtoVND và VNDtoUSD đều gửi dữ liệu lên component App thông quá {props.value}.
    Từ value đó component App sẽ lưu vào state của App rồi gửi về component còn lại thông qua value = {this.state.usd hoặc vnd}
### Composition vs Inheritance
    Trong react, từ 1 giao diện trang web ta có thể chia nhỏ ra thành rất nhiều component kết hợp lại với nhau
    Việc chia nhỏ này sẽ giúp bạn dễ quản lí component hơn và hiệu năng nhanh hơn thay vì phải reload lại trang thường xuyên như khi sử dụng HTML
    Các Component có thể nhận vào bất kì Props nào bao gồm giá trị cơ bản, React Element hay có thể là cả function. Ngoải ra nếu bạn muốn sử dụng function giữa các component bạn có thể chỉ cần dùng export và import mà k cần phải extend
    Và component không sử dụng tính kế thừa
```javascript
    // đây là cách Component có thể nhận vào bất kì kiểu dữ liệu gì thông qua props 
    function SplitPane(props) {
        return (
            <div className="SplitPane">
                <div className="SplitPane-left">
                    {props.left}
                </div>
                <div className="SplitPane-right">
                    {props.right}
                </div>
            </div>
        );
    }

    function App() {
        return (
            <SplitPane
                left={
                    <Contacts />
                }
                right={
                    <Chat />
            } />
        );
    }
    // đây là ví dụ về cách thay đổi data dựa vào input đầu vào
    function Dialog(props) {
        return (
            <FancyBorder color="blue">
                    <h1 className="Dialog-title">
                    {props.title}           
                    </h1>
                    <p className="Dialog-message">
                    {props.message}
                </p>
            </FancyBorder>
        );
    }
    // props.title sẽ nhận giá trị "Welcome"
    // props.message sẽ nhận giá trị "Thank you for visiting our spacecraft!"
    function WelcomeDialog() {
        return (
            <Dialog
                title="Welcome"  
                message="Thank you for visiting our spacecraft!" 
            />
        );
    }
```
### Thingking in React
#### Chia UI thành Component theo bậc
#### Tối ưu code ở mức tối thiểu nhưng phải đầy đủ
#### Sử dụng hợp lí state props






