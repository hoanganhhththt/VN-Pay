# Routing
## React-Router
    - Là 1 thư viện định tuyến tiêu chuẩn của React
    - Giữ cho giao diện đồng bộ vs URL và cho phép định tuyến "luông dữ liệu" trong ứng dụng 1 cách rõ ràng
    - Cách cài đặt npm install react-route-dom --save
    - Gồm có 3 thành phần : BrowserRouter, Route, Link
## BrowserRouter
    - Dùng để gói tất cả route component
    - Import từ react-router-dom và sử dụng
```javascript
    import React from 'react'
    import ReactDOM from 'react-dom'
    import { BrowserRouter as Router } from 'react-router-dom'
    // BrowserRouter chỉ có thể chứa 1 component con nên ta có thể gói trong 1 thẻ div
    ReactDOM.render(
        <Router>
            <div>
                ...
            </div>
        </Router>
    )
```
## Route
    Dùng để ẩn hiện Component mà nó chứa. Nó sẽ render ra component ứng vs path tương ứng
    Nếu k ứng vs path nào thì nó sẽ return null
```javascript
    import React from 'react'
    import ReactDOM from 'react-dom'
    import { BrowserRouter as Router, Route } from 'react-router-dom'

    const HomePage = () => (
        <div>
            <h1>Trang chủ</h1>
        </div>
    )

    const ItemPage = () => (
        <div>
            <h1>Trang item</h1>
        </div>
    )

    ReactDOM.render(
        <Router>
            <div>
                <Route path='/' exact component={HomePage} />
                <Route path='/items' component={ItemPage} />
            </div>
        </Router>,
        document.getElementById('app')
    )
```
    Trong đó:
    - path: là đường dẫn trên URL
    - exact: Giúp cho route này chỉ hoạt động nếu URL trên trình duyệt phù hợp tuyệt đối với giá trị của thuộc tính path của nó.
    - component: là component sẽ được render ứng vs Route đó
## Link
    DÙng để chuyển trang
### Link
```javascript
    <Link to="/items">Trang Item</Link> 
```
    to giống như thuộc tính href của thẻ a HTML
### NavLink
```javascript
    <NavLink exact activeStyle={{
        backgroundColor : 'white',
        color : 'red'
        }} 
        to="/" 
        className="my-link">
        Trang Chu
    </NavLink>
```
    Giống vs thẻ Link về chức năng tuy nhiên nó có bổ sung thêm thuộc tính activeClassName và activeStyle2. Khi component được gọi ra thì nó sẽ được set style sẵn
## Prompt
    Nó sẽ gửi yêu cầu xác nhận của bạn rồi mới chuyển trang
```javascript
    import {Prompt} from 'react-router-dom';

    <Prompt 
        when={true} // true | false
        message={ (location) => (`Ban chac chan muon di den ${location.pathname}`) }
    />
```
## Redirect
    - Chức năng dùng để chuyển trang
    - Có thể truy xuất thông tin trang trước đó thông qua đối tượng location.
```javascript
    <Redirect to={{
        pathname : '/products',
        state : {
            from : location
        }
    }} />
```

