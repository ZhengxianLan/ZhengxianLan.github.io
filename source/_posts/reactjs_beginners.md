---
title: reactjs fish
layout: post
date: 2016-07-24 12:34:56
categories: ['js']
tags: ['react']
---

-. 只是一个 view lib
-. 保持组件轻量
-. 书写函数式组件
```javascript
const MyComponent = React.createClass({render: function() {return <div className={this.props.className}/>;
  }
});
```
```javascript
class MyComponent extends React.Component {render() {return <div className={this.props.className}/>;
  }
}
```
```javascript
const MyComponent = props => (<div className={props.className}/>
);
```
举个例子
```javascript
class LatestPostsComponent extends React.Component {render() {const postPreviews = renderPostPreviews();

    return (<section>
        <div><h1>Latest posts</h1></div>
        <div>
          {postPreviews}
        </div>
      </section>
    );
  }

  renderPostPreviews() {return this.props.posts.map(post => this.renderPostPreview(post));
  }

  renderPostPreview(post) {
    return (<article>
        <h3><a href={`/post/${post.slug}`}>{post.title}</a></h3>
        <time pubdate><em>{post.posted}</em></time>
        <div>
          <span>{post.blurb}</span>
          <a href={`/post/${post.slug}`}>Read more...</a>
        </div>
      </article>
    );
  }
}
```
```javascript
const LatestPostsComponent = props => {const postPreviews = renderPostPreviews(props.posts);

  return (<section>
      <div><h1>Latest posts</h1></div>
      <div>
        {postPreviews}
      </div>
    </section>
  );
};

const renderPostPreviews = posts => (posts.map(post => this.renderPostPreview(post))
);

const renderPostPreview = post => (<article>
    <h3><a href={`/post/${post.slug}`}>{post.title}</a></h3>
    <time pubdate><em>{post.posted}</em></time>
    <div>
      <span>{post.blurb}</span>
      <a href={`/post/${post.slug}`}>Read more...</a>
    </div>
  </article>
);

```
- 上面功能一样的代码，一个是写在类内的几个方法，一个是独立的几个函数。这样更容易将大组件拆分为小组件.
 不过函数式的组件，有 2 个限制： 
  1. 不能又 ref 属性. 
  2. 不能绑定状态
- 不过，这些限制并非坏事: 
  1. 直接操作 dom 不是 react 的风格; 
  2. 状态是痛苦的主要来源
- 书写无状态的组件
  - 状态使得组件难于测试
  - 状态使得组件难以理解
  - 状态使得业务逻辑易于混入组件
  - 状态使得和 app 其他部分分享信息变得困难
- 使用 redux.js
- 常使用 propTypes
- 使用浅渲染
- 使用 JSX,ES6,Babel,NPM
- 使用 React 和 Redux dev tools


