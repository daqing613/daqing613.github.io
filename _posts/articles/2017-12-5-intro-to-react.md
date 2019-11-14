---
layout: post
title: "Introduction to React"
excerpt: "用来创建用户界面的JavaScript库" 
categories: articles
tags: ['React', 'JavaScript', 'JSX']
comments: true
share: true
image:
  feature: so-simple-sample-image-2.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---


# React 

是用来创建用户界面的一个JavaScript库。 

## 特性

1. *陈述*  React使创建交互式UI不在痛苦。 为你的程序的不同状态设计简单的视图， React会在你的数据改变时高效得更新和渲染相应的组件。 

   陈述式视图是你的代码更加可预见性和易于排错。 

2. *组件为基础的*  创建管理自己状态的封装组件，使他们组成复杂的用户界面。 

   自从组件逻辑是写在JavaScript里面而不是templates中， 你可以轻松得通过你的app来传递丰富的数据同时保持状态远离DOM。 

3. *一次学习， 到处写*  React不会假设你其他的技术栈， 所以你不需要重写已经存在的代码来研发出新的特色。 

## 案例 

### 一个简单的组件

React组件实现了render的方法， 它接受输入的数据同时返回需要显示的东西。 这个例子使用的是一个被称为JSX的XML-like语法。输入到组件的数据可以通过**this.props**被**render()**访问。 



**JSX是可选的同时也不是要求得来使用React.** 尝试[Babel REPL](https://babeljs.io/repl/#?presets=react&code_lz=MYewdgzgLgBApgGzgWzmWBeGAeAFgRgD4AJRBEAGhgHcQAnBAEwEJsB6AwgbgChRJY_KAEMAlmDh0YWRiGABXVOgB0AczhQAokiVQAQgE8AkowAUPGDADkdECChWeASl4AlOMOBQAIgHkAssp0aIySpogoaFBUQmISdC48QA)来看JSX编译步骤生成的JavaScript源码。 

```jsx
class HelloMessage extends React.Component {
  render() {
    return (
      <div>
        Hello {this.props.name}
      </div>
    );
  }
}

ReactDOM.render(
  <HelloMessage name="Taylor" />,
  mountNode
);
```

### 一个有状态的组件

除了接收输入数据(通过**this.props**访问)， 一个组件也可以维护内部状态数据(通过**this.state**访问)。 当一个组件的状态数据改变， 渲染的备份会被重新调用**render()**来更新。 

```jsx
class Timer extends React.Component {
  constructor(props) {
    super(props);
    this.state = { seconds: 0 };
  }

  tick() {
    this.setState(prevState => ({
      seconds: prevState.seconds + 1
    }));
  }

  componentDidMount() {
    this.interval = setInterval(() => this.tick(), 1000);
  }

  componentWillUnmount() {
    clearInterval(this.interval);
  }

  render() {
    return (
      <div>
        Seconds: {this.state.seconds}
      </div>
    );
  }
}

ReactDOM.render(<Timer />, mountNode);
```



### 一个应用

使用props和state， 我们组合一起成为一个小的Todo应用程序。 这个例子使用**state**来追踪当前项目的列表和用户输入的文本。 即使事件控制器好像是在行内渲染， 他们使用事件代理来手机和实施。 

```jsx
class TodoApp extends React.Component {
  constructor(props) {
    super(props);
    this.state = { items: [], text: '' };
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  render() {
    return (
      <div>
        <h3>TODO</h3>
        <TodoList items={this.state.items} />
        <form onSubmit={this.handleSubmit}>
          <input
            onChange={this.handleChange}
            value={this.state.text}
          />
          <button>
            Add #{this.state.items.length + 1}
          </button>
        </form>
      </div>
    );
  }

  handleChange(e) {
    this.setState({ text: e.target.value });
  }

  handleSubmit(e) {
    e.preventDefault();
    if (!this.state.text.length) {
      return;
    }
    const newItem = {
      text: this.state.text,
      id: Date.now()
    };
    this.setState(prevState => ({
      items: prevState.items.concat(newItem),
      text: ''
    }));
  }
}

class TodoList extends React.Component {
  render() {
    return (
      <ul>
        {this.props.items.map(item => (
          <li key={item.id}>{item.text}</li>
        ))}
      </ul>
    );
  }
}

ReactDOM.render(<TodoApp />, mountNode);
```



### 一个使用外部插件的组件 

React是一个灵活地同时提供钩子来允许你与其他库和框架来链接。 这个例子使用**remarkable**, 一个外部的Markdown 库， 来实时转换\<textarea>的值。 

```bash
class MarkdownEditor extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = { value: 'Type some *markdown* here!' };
  }

  handleChange(e) {
    this.setState({ value: e.target.value });
  }

  getRawMarkup() {
    const md = new Remarkable();
    return { __html: md.render(this.state.value) };
  }

  render() {
    return (
      <div className="MarkdownEditor">
        <h3>Input</h3>
        <textarea
          onChange={this.handleChange}
          defaultValue={this.state.value}
        />
        <h3>Output</h3>
        <div
          className="content"
          dangerouslySetInnerHTML={this.getRawMarkup()}
        />
      </div>
    );
  }
}

ReactDOM.render(<MarkdownEditor />, mountNode);	
```



## 引用

如果你想了解更详细的内容，请访问官方文档： 

> https://reactjs.org/tutorial/tutorial.html	
