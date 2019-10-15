---
title: React开发规范1.0
date: 2018-03-05 10:43:39
tags: [前端,React]
categories: 规范
---
## React 代码规范

### 内容目录 

[Toc]

---
>1.部分遵循 [eslint 规则](https://github.com/yannickcr/eslint-plugin-react/tree/master/docs/rules)， 后期可以直接引入 eslint
>
>2.此规范参考 tinper-bee 及 airbnb react 开发规范 

### 基础规范
- 统一使用 `utf-8` 编码
- ==回车换行==统一使用`CRLF`
- `Tab` 缩进为 `4` 个空格
- 统一使用 ES6 语法进行开发 [ES6 语法参考](http://es6.ruanyifeng.com/)
- 使用 JSX 语法，模块文件统一使用 js 后缀
- 不要使用 `React.createClass`
- 每一个文件只包含一个组件，每一个基本组件只包含单一功能 [组件设计原则](https://github.com/tinper-bee/react-components-docs/blob/master/%E7%BB%84%E4%BB%B6%E5%88%92%E5%88%86%E5%8E%9F%E5%88%99.md)

<!--more-->
### 命名
- #### 文件命名
    - 文件名使用大驼峰命名 `MyComponent`

- #### 模块命名
    - 模块使用当前模块文件夹名一样的名称
    > 如果整个文件夹是一个模块，使用 `index.js`作为入口文件，然后直接使用当前文件夹名作为模块名称

    ```javascript
    // bad
    import Footer from './Footer/index';

    // good
    import Footer from './Footer';
    ```

- #### 引用命名
    - 模块名使用大驼峰命名

    ```javascript
    // bad
    import myComponent from './MyComponent';

    // good
    import MyComponent from './MyComponent';
    ```
    - 实例使用正常驼峰命名
    ```jsx
    // bad
    const MyComponent = <MyComponent />;

    // good
    const myComponent = <MyComponent />;
    ```
- #### 属性命名
    - 避免使用 DOM 相关属性来用作其他用途
    > 相关属性为正常 HTML 标签属性，如 `style、id、class、href` 等

    ```jsx
    // bad
    <MyComponent style="fancy" />

    // good
    <MyComponent variant="fancy" />
    ```

### JSX 书写规范
- #### 代码对齐
    - 遵循以下的 JSX 语法缩进格式
    ```jsx
    // bad
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />
    
    // good, 有多行属性的话, 新建一行关闭标签
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />
    
    // 若能在一行中显示, 直接写成一行
    <Foo bar="bar" />
    
    // 子元素按照常规方式缩进
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
    </Foo>
    ```
- #### 单双引号
    - 对于 JSX 属性值总是使用双引号（`"`）,其他均使用单引号（`'`）
    ```jsx
    // bad
    <Foo bar='bar' />
    
    // good
    <Foo bar="bar" />
    
    // bad
    <Foo style={{ left: "20px" }} />
    
    // good
    <Foo style={{ left: '20px' }} />
    ```
- #### 空格
    - 总是在自动关闭的标签前加一个空格，正常情况下不需要换行
    ```jsx
    // bad
    <Foo/>
    
    // very bad
    <Foo                 />
    
    // bad
    <Foo
     />
    
    // good
    <Foo />
    ```
    - 不要在 JSX `{}` 引用括号里两边加空格
    ```jsx
    // bad
    <Foo bar={ baz } />
    
    // good
    <Foo bar={baz} />
    ```

- #### 标签
    - 对于没有子元素的标签来说总是自己关闭标签
    ```jsx
    // bad
    <Foo className="stuff"></Foo>

    // good
    <Foo className="stuff" />
    ```
    - 如果模块有多行的属性，关闭标签时新建一行
    ```jsx
    // bad
    <Foo
        bar="bar"
        baz="baz" />

    // good
    <Foo
        bar="bar"
        baz="baz"
    />
    ```
- #### 组件结构

    - 总体规则

        函数式组件形式优于 ES6 Class

    - 书写规则

        组件（ES6 Class）内部生命周期函数书写顺序，如下

    1. 可选的`static`方法
    
    2. `constructor`构造函数
    
    3. `getChildContext`获取子元素内容
    
    4. `componentWillMount`模块渲染前
    
    5. `componentDidMount`模块渲染后
    
    6. `componentWillReceiveProps`模块将接受新的数据
    
    7. `shouldComponentUpdate`判断模块需不需要重新渲染
    
    8. `componentWillUpdate`上面的的方法返回 true ，模块将重新渲染
    
    9. `componentDidUpdate`模块渲染结束
    
    10. `componentWillUnmount`模块将从DOM中清除，可以做些清理任务
    
    11. 点击回调或者事件处理器 如 `onClickSubmit()` 或 `onChangeDescription()`
    
    12. `render` 里的 getter方法 如 `getSelectReason()` 或 `getFooterContent()`
    
    13. 可选的 render 方法 如 `renderNavigation()` 或 `renderProfilePicture()`
    
    14. `render` render() 方法
    
        - 示例代码
        > 如何定义 propTypes, defaultProps, contextTypes, 等其他属性...
        
        ```javascript
        import React from 'react';
        import PropTypes from 'prop-types';
        
        // 定义 props 类型
        const propTypes = {
          name: React.PropTypes.string
        };
        
        // 如果需要
        const defaultProps = {
          name: 'Guest'
        };
        
        class Person extends React.Component {
          // 构造函数
          constructor (props) {
            super(props);
            // 定义 state
            this.state = { smiling: false };
          }
        
          // 生命周期方法
          componentWillMount () {},
          componentDidMount () {},
          componentWillUnmount () {},
          
          // getters and setters
          get attr() {}
        
          // handlers
          handleClick = ()=>{},
        
          // render
          renderChild() {},
          render () {},
        
        }
        
        /**
         * 类变量定义
         */
        Person.defaultProps = defaultProps;
        
        /**
         * 统一都要定义 propTypes
         * @type {Object}
         */
        Person.propTypes = propTypes;
        ```
<!--
- #### Refs 
    - 在 Refs 里使用回调函数

    ```jsx
    // bad
    <Foo
      ref="myRef"
    />
    
    // good
    <Foo
      ref={(ref) => { this.myRef = ref; }}
    />
    ```
-->
### 语法规范
- #### return (返回)
    - 将多行的 JSX 标签写在 `()` 里，并确保有返回内容

    ```javascript
    // bad
    render() {
      return <MyComponent className="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }
    
    // good
    render() {
      return (
        <MyComponent className="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }
    
    // good,单行可以不需要
    render() {
      const body = <div>hello</div>;
      return <MyComponent>{body}</MyComponent>;
    }
    ```

<!--

### 测试相关

- 不推荐对鼠标右键功能进行改写
- 页面各主要元素应设定唯一性标识
    > 标识对照如下表
    
    分类     | 标识     | 来源      
    ---|---|---
    input  | name   |   可以使用后台表中定义页面展示元素字段名或id
    button | id     | 根据按钮操作功能进行定义
    a   | href  | 根据 href 属性确定 a 标签的唯一性
    其他容器标签（带事件）|id   | 根据当前容器事件功能进行定义
    -->



