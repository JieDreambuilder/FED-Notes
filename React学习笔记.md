### React Notes
* 在`JSX`语法中，标签的`class`属性需要写成`className`，`for`属性要写成`htmlFor`。因为`class`和`for`是`JavaScript`的保留字。
* 标签的事件名都要写成驼峰规则：
    * 平时我们写的时候都是：`<p onclick='handleClick'>`
    * 在`JSX`中要写成：`<p onClick='this.handleClick'>`
* 组件类的第一个字母必须大写，否则会报错。
    ```javascript
    var HelloWord=React.creatClass({
        render:function(){
            return <h1>hello world!</h1>
        }
    });
    ```
* 组件类只能包含一个顶层标签，否则也会报错。
    ```javascript
    var HelloWord=React.creatClass({
        render:function(){
            return <h1>hello world!</h1><p>Jhon</p>    //false!
        }
    });
    ```
* 关于注释：
    * 需要写在花括号中
    ```javascript
    ReactDOM.render(
        <div>
        <h1>菜鸟教程</h1>
        {/*注释...*/}
         </div>,
        document.getElementById('example')
    );
    ```
    * 原因：
        * JSX遇到 HTML 标签（以 `<`开头），就用 HTML 规则解析；遇到代码块（以 `{`开头），就用 JavaScript 规则解析。
        * `HTML`的注释和`JS`的注释不一样。
            * 前者是这样的：`<!-- 注释 -->`
            * 后者是这样的：`/* 注释 */`
        * 所以当需要在标签里写`JS`的东西的时候，都需要加`{}`。
* 组件状态：
    * 初始化组件状态的方法是`getInitialState`,组件加载时会调用这个属性下的方法，把返回值赋给组件的`state`属性。
    * 设置组件状态需要调用`this.setState`方法。参数是一个对象，对象内容即为组件状态的内容。
    * 状态发生更改时，会重新调用`render`渲染。
    ```javascript
    var ButtonLike=React.createClass({
        getInitialState:function () {
            return {liked:false};
        },
        handleMouseOver:function () {
            this.setState({liked:!this.state.liked})
        },
        render:function () {
            var text=this.state.liked?'喜欢':'不喜欢';
            return <button onMouseOver={this.handleMouseOver}>{text}</button>
        }
    });
    ```
* 组件的属性可以在组件类的`this.props`对象上获取。
    ```javascript
    var HelloMessage = React.createClass({
      render: function() {
        return <h1>
          Hello {this.props.name}
        </h1>;
      }
    });
    ReactDOM.render(<HelloMessage name='Jhon' />,document.getElementById('example'));
    
    // <h1 name='Jhon'>Hello Jhon</h1>
    ```
* `this.props.children`:
    * `this.props.children`属性表示组件的所有子节点。
    * `this.props.children`的值有三种可能：如果当前组件没有子节点，它就是 `undefined`;如果有一个子节点，数据类型是`object` ；如果有多个子节点，数据类型就是`array`。
    * 用`React.Children.map`来遍历子节点：
    ```javascript
    var NotesList = React.createClass({
      render: function() {
        return (
          <ol>
          {
            React.Children.map(this.props.children, function (child) {
              return <li>{child}</li>;
            })
          }
          </ol>
        );
      }
    });
    
    ReactDOM.render(
      <NotesList>
        <span>hello</span>
        <span>world</span>
      </NotesList>,
      document.body
    );
    
    //输出：
    //<ol>
    //  <li><span>hello</span></li>
    //  <li><span>world</span></li>
    //</ol>
    ```
* `PropTypes`:用来验证组件实例的属性是否符合要求。
    ```javascript
    var MyTitle = React.createClass({
      propTypes: {
        title: React.PropTypes.string.isRequired,
      },
    
      render: function() {
         return <h1> {this.props.title} </h1>;
       }
    });
    
    var data = 123;

    ReactDOM.render(
      <MyTitle title={data} />,
      document.body
    );
    
    //Warning: Failed propType: Invalid prop `title` of type `number` supplied to `MyTitle`, expected `string`.
    ```
    更多`PropTypes`设置可查看[官网](http://facebook.github.io/react/docs/reusable-components.html)

* `getDefaultProps`用来设置默认属性：
    ```javascript
    var MyTitle = React.createClass({
      getDefaultProps : function () {
        return {
          title : 'Hello World'
        };
      },
    
      render: function() {
         return <h1> {this.props.title} </h1>;
       }
    });
    
    ReactDOM.render(
      <MyTitle />,
      document.body
    );
    
    //  <h1>Hello World</h1>
    ```
* `ref`获取真实的`DOM`节点:
    * 因为`React`采用的时虚拟`DOM`渲染，只有当组件插入文档以后，才会变成真实的 `DOM`。虚拟 DOM 是拿不到用户输入的，需要操作真实的`DOM`元素的时候就要使用`ref`属性：
    ```javascript
        var MyComponent = React.createClass({
      handleClick: function() {
        this.refs.myTextInput.focus();
      },
      render: function() {
        return (
          <div>
            <input type="text" ref="myTextInput" />
            <input type="button" value="Focus the text input" onClick={this.handleClick} />
          </div>
        );
      }
    });
    
    ReactDOM.render(
      <MyComponent />,
      document.getElementById('example')
    );
    ```
    * 被操作元素必须有一个`ref`属性，然后在`JS`中通过`this.refs.[refName]`来获取。
* 表单的事件不能通过`this.props.value`获取，需要通过回调函数处理：
    ```javascript
        var Input = React.createClass({
      getInitialState: function() {
        return {value: 'Hello!'};
      },
      handleChange: function(event) {
        this.setState({value: event.target.value});
      },
      render: function () {
        var value = this.state.value;
        return (
          <div>
            <input type="text" value={value} onChange={this.handleChange} />
            <p>{value}</p>
          </div>
        );
      }
    });
    
    ReactDOM.render(<Input/>, document.body);
    ```
* 组件的生命周期：
    * `Mounting`：已插入真实 DOM
        * `componentWillMount()`:插入DOM前调用
        * `componentDidMount()`:插入DOM后调用
    * `Updating`：正在被重新渲染
        * `componentWillUpdate(object nextProps, object nextState)`:更新前调用
        * `componentDidUpdate(object prevProps, object prevState)`:更新后调用
    * `Unmounting`：已移出真实 DOM
        * `componentWillUnmount()`:移除前调用
    * `componentWillReceiveProps(object nextProps)`：已加载组件收到新的参数时调用，在初始化`render`时不会被调用
    * `shouldComponentUpdate(object nextProps, object nextState)`：返回一个布尔值。在组件接收到新的props或者state时被调用。在初始化时或者使用forceUpdate时不被调用。可以在你确认不需要更新组件时使用。
    ```javascript
    var Hello = React.createClass({
      getInitialState: function () {
        return {
          opacity: 1.0
        };
      },
    
      componentDidMount: function () {
        this.timer = setInterval(function () {
          var opacity = this.state.opacity;
          opacity -= .05;
          if (opacity < 0.1) {
            opacity = 1.0;
          }
          this.setState({
            opacity: opacity
          });
        }.bind(this), 100);
      },
    
      render: function () {
        return (
          <div style={{opacity: this.state.opacity}}>
            Hello {this.props.name}
          </div>
        );
      }
    });
    
    ReactDOM.render(
      <Hello name="world"/>,
      document.body
    );
    ```
* `React`组件样式是一个对象，所以第一重大括号表示这是`JavaScript` 语法，第二重大括号表示样式对象。
    ```javascript
    <div style={{opacity: this.state.opacity}}>
    ```
* Ajax：在`componentDidMount`中请求数据，然后通过设置`this.setState()`方法触发`UI`的重新渲染。
    * `JQuery`例子：
    ```javascript
    var UserGist = React.createClass({
      getInitialState: function() {
        return {
          username: '',
          lastGistUrl: ''
        };
      },
    
      componentDidMount: function() {
        $.get(this.props.source, function(result) {
          var lastGist = result[0];
          if (this.isMounted()) {
            this.setState({
              username: lastGist.owner.login,
              lastGistUrl: lastGist.html_url
            });
          }
        }.bind(this));
      },
    
      render: function() {
        return (
          <div>
            {this.state.username}s last gist is
            <a href={this.state.lastGistUrl}>here</a>.
          </div>
        );
      }
    });
    
    ReactDOM.render(
      <UserGist source="https://api.github.com/users/octocat/gists" />,
      document.body
    );
    ```
    * `Permise`例子：
    ```javascript
    var RepoList = React.createClass({
      getInitialState: function() {
        return { loading: true, error: null, data: null};
      },
    
      componentDidMount() {
        this.props.promise.then(
          value => this.setState({loading: false, data: value}),
          error => this.setState({loading: false, error: error}));
      },
    
      render: function() {
        if (this.state.loading) {
          return <span>Loading...</span>;
        }
        else if (this.state.error !== null) {
          return <span>Error: {this.state.error.message}</span>;
        }
        else {
          var repos = this.state.data.items;
          var repoList = repos.map(function (repo) {
            return (
              <li>
                <a href={repo.html_url}>{repo.name}</a> ({repo.stargazers_count} stars) <br/> {repo.description}
              </li>
            );
          });
          return (
            <main>
              <h1>Most Popular JavaScript Projects in Github</h1>
              <ol>{repoList}</ol>
            </main>
          );
        }
      }
    });
    
    ReactDOM.render(
      <RepoList
        promise={$.getJSON('https://api.github.com/search/repositories?q=javascript&sort=stars')}
      />,
      document.body
    );
    ```
------------------------------
* 更多参考：
    * 阮一峰老师的[《React入门教程》](http://www.ruanyifeng.com/blog/2015/03/react.html)
    * [菜鸟React教程](http://www.runoob.com/react/react-tutorial.html)
    * [React 官网](https://facebook.github.io/react/)