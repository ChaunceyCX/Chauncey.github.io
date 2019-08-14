# 组件

## 组件实现:
    1. 定义组件
       1. 工厂函数组件,效率高(如果组件中定义了状态就不能使用此模式)

```js
function MyComponebt() {
    return <h2></h2>
}
```
        2. ES6
   
```js
class MyComponent2 extends React.Component {
    render () {
        return <h2></h2>
    }
}
```
    2. 渲染组件标签

```js
//工厂函数渲染
ReactDom.render(<MyComponent/>,document.getElementById('xxx'))
//class
ReactDom.render(<MyComponent2/>,document.getElementById('xxx'))
```

## 组件三大属性

1. state(组件内定义)
2. props(组件外定义,渲染时传入)

```js

function Person(props) {
    return <h2>{props.name}<h2>
}

//props默认值
Person.defaultProps = {
    name: 'xxx'
}

//props 指定参数类型(需要引入相应包)
Person.propTypes = {
    name: PropTypes.string.isRequired
}

ReactDom.render(<Person {pa.name}>,document.getElementById('xxx'))

```

3. ref和事件处理

```js
class MyComponrnt extends React.Compontent {

    construstor (props) {
        super(props)

        this.show = this.show.bind(this)
        this.alartInput = this.alartInput.bind(this)
    }

    show() {
        //不推荐方式
        const input = this.refs.content
        alart(input.value)
        //推荐方式
        alart(this.input.value)
    }

    alartInput (event) {
        alart(event.target.value)
    }

    render() {
        return (
            <div>
                //官方不推荐
                <input type="text" ref="content"/>
                //将当前input标签所代表的元素给this
                <input type="text" ref={input => this.input = input}/>
                <button onClick={this.show}>弹出输入值</button>
                
                <input type="text" placeholder="失去焦点弹窗" onBlur={this.alartInput}/>
            </div>
        )
    }
}
```

## 组合组件(TODO list作为例子)

```js
class Add extends React.Component {

    constructor (props) {
        super(props)
        this.add = this.add.bind(this)
    }

    addTodo() {
        const input = this.input.value
        this.props.add(input)
    }

    render(props) {
        return (
            <div>
                <input type="text" ref={input => this.input=input}/>
                <button onClick={this.addTodo}>添加{props.cont}</button>
            </div>
        )
    }

    Add.propTypes = {
        cont: PropTupes.number.isRequired,
    }
}

class List extends React.Component {

    render(props) {
        return (
            <ul>
                this.props.todos.map((todo,index) => { return <li key={index}> {todo} </li>})
            </ul>
        )
    }

    List.propTypes = {
        todos: PropsTypes.array.isrequired
    }
}

class App extends React.Component {

    constructor(props) {
        super(props)

        this.state = {
            todos: ["吃饭", "睡觉", "写代码"]
        }

        this.add = this.add.bind(this)
    }

    add(todo) {
        const {todos} = this.state
        todos.unshift(todo)
        this.setState({todos})
    }

    render() {
        const {todos} = this.state
        return (
            <div>
                <Add cont={todos.length} add={this.add} />
                <List todos={todos}/>
            </div>
        )
    }
}

```

## 受控组件/非受控组件

```js
class MyComponent extends React.Component{

    constructor(props) {
        super(props)
        
    }

    onPwdChange(event){
        const pwd = event.target.value
        this.setState({pwd})
    }

    render() {
        return (
            <div>
                //非受控组件
                <input type="text" refs={input => this.input=input} />
                //受控组件
                <input type="password" value= {this.state.pwd} onchange={this.onPwdChange}/>
            </div>
        )
    }
}
```

## 组件生命周期

1. 第一次初始化渲染显示:ReactDOM.render()
   - constructor():创建对象初始化state
   - componentWillMount():将要挂在时的回调函数
   - render():插入虚拟DOM
   - componentDidMount():已经挂在的回调函数
2. 每次更新state:this.setState()
   - componentWillUpdate():将要更新回调
   - render():更新渲染
   - componentDidMount():已经更新的回调
3. 移除组件:ReactDOM.unmountComponentAtNode(document.getElementById("xxx"))
   - componentWillUnmount():组件将要被移除回调

### 重要的钩子

1. render():初始化渲染或更新渲染
2. componentDidMount():开启监听发送ajax请求,等异步操作
3. componentWillUnmount(): 做一些收尾工作:如:清理定时器
4. compomemtWillReceiveProps()

