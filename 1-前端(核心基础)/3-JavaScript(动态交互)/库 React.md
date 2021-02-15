JavaScript的库：用来创建用户界面

- robust
- 可重用

由Facebook创造，用来开发单页面应用SPA(single page applications)

- SPA: 加载新的内容时，不会刷新的网页



三个重要概念：

- components：组合用户界面的元素/blocks
- JSX：扩展到JavaScript
- props

```react
//类和继承
class Dog extends Animal {
    
}
```



## components

```react
//components
class Greet extends Component {
    render() {	//在render()内可添加GTML
        return(<div>Hi there</div>)
    }
}


ReactDOM.render((
	<Greet/>
), document.getElementById('root'));
```



对应的HTML：

```html
<div id="root">
    
</div>
```



## JSX

上述的`<Greet/>`就是JSX

使得react组件对应的JavaScript和HTML更好的结合





```react
class List extends Component {
    render() {
        <ul>
            <li></li>
        	<li></li>
        </ul>
    }
}
```





## props: properties

类似参数传递给components

在components中嵌套components

```react
class Greet extends Component {
    render() {	//在render()内可添加GTML
        return(
            <div>Hi there</div>		//配套：Hi {this.props.name}
        )
    }
}


class Page extends Component {
    render() {
        return(
        <div>
                <Greet name="Tina"/>	//<Greet user="Helen"/>
        </div>
        )
    }
}
```



多个参数：

```react
class Results extend Component {
    render() {
        return(
            <div>
            	<Places first="Ann" second="Joe"/>
            </div>
        )
    }
}
    
class Board extends Component {
    render() {
        return(
        <div>
            First {this.props.first}
            Second {this.props.second}
        </div>
        )
    }
}
```



一般情况下，网页需要使用DOM检测CSS和布局的变化。

但是在React中使用virtual DOM：只检测与前一个版本的差异





```react
class App extends Component {
    render() {
        return(
        	<List fruit="Kiwi" veg="Kale"/>	//这就是两个prop
        )
    }
}

class List extends Component {
    render() {
        <ul>
            <li>{this.props.veg}</li>
        	<li>{this.props.fruit}</li>
        </ul>
    }
}
```





# #me|理解

可能是抽离出变量和内容，方便统一设置