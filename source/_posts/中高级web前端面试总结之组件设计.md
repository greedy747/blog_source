---
title: 中高级web前端面试总结之组件设计
date: 
category: web前端
tags: 
    - 前端面试
---
MVVM框架的流行让web前端的开发更加专注于数据和逻辑，而很少亲自操作dom来处理视图，同时，组件化也是MVVM框架带来重要特性之一，是中高级前端面试基本都会考察的一个点。本文不会过多地讨论组件化设计的理念或者思路(因为本人还不够格，哈哈哈)，重点是结合几场面试经历来讲讲要怎么回答这类问题。
<!-- more -->
## 一、问题
一般面试进行到后面几轮，基本上都会问这个问题，这也确实一个好问题。一般的提问形式会是，“你对组件设计有什么经验”或者“如果要你设计一个XX组件，你会怎么做”。或者直接给出详细需求，要求面试者直接手写出来。很明显，这个问题主要考察面试者是否有足够的组件实战经验或者设计心得。
<img src="/images/blackface.jpg" width="250px" height="250px">
然而，虽然工作中写了不少组件，当真正被问到这种问题的时候，还是会有些不知所措，因为面试中问到的往往都是非常基础的组件，比如级联组件、轮播等。但是实际业务中，这些基础组件通常都会使用开源的或者现成的，很多都没有自己去实现过（就算是面试官他自己也一样吧）。而对于实际的业务组件，以我的经验来看，更多的是一些UI上的封装组合，偶尔会有一些功能的mixin。所以，在一个短时间里要回答好这种问题，并不是那么容易，尤其碰到一些比较“执着”的面试官，下面我会举一个例子。

## 二、例子(某巢2面)
Q：如果要设计一个自动适应宽高的输入框组件，要怎么设计（开门见山，面试官一看就是奋战在编码一线的老码农）
A：emmm。。。好像一下子没啥思路。。。
Q: 你可以讲一下你想到的东西，比如要设计哪些接口啊，怎么封装啊？
A：emmm。。。这个组件应该可以接收一些关于样式上的props，还会有个开关属性来启用这个功能，blablabla.....至于这个自动适应的功能，我还没啥想法。。（然后面试官开始引导我，然后我越来越蒙。。。）
Q：那我们换一个，有没有封装过list组件？
A：什么list组件，是那种list容器吗，接收一个组件作为要展示的内容，容器本身具有loadmore之类的功能？
Q：差不多，但是我们的要展示的组件可能有多种，比如banner啊，文本啊，图片啊，list组件需要根据返回的数据来渲染对应的组件
A：emmm。。。那这些组件的模板是固定的吗，还是后台返回的?
Q：后台返回的。
A：emm...如果是基于vue的组件的话，应该是要用到作用域插槽，blabla（已经不知道自己在讲啥。。。）
Q：这个问题呢，还可以问的更深，比如如何保证这个组件的性能（我猜面试官想问，高性能无限滚动？），那讲一下常用的那种级联组件是怎么实现？
A：这个我没自己实现过，应该要接收一个data数组，结构大概是这样的，blabla....组件提供的事件接口包括一个selectChange吧，实应该会用到递归吧，其他的api，应该就是一些UI方面的吧，比如组件大小啊什么的。blabla...

从以上的面试过程来看，面试官很看重面试者组件设计的能力，准备也很充分，可以一直问到面试者回答不出来。面试官还给我讲了他如何重新写了一个轮播组件来兼容低版本安卓机卡顿的问题（低版本安卓机无法利用css开启硬件加速，用浏览器原生支持的scroll来实现？）。对于没有从0实现过这些组件的人来说，实在是不容易啊。当然了，对于面试官的穷追猛打，本人并不是完全赞同。实际上，在面试过程中，很多时候我不能完全get到面试官的点，泛泛而谈的话好像显得没有真才实学，具体到实处回答的话，又好像要说的细节太多，容易坑了自己。

## 三、拆招要诀
#### 3.1 内功心法
虽然不能把那么多常见的组件一一亲自实现，但是基本上组件设计都会遵循一定的原则或者最佳实践，比如以下这些
* 单一职责
* 粒度尽量小
* Container and Presentational pattern(react 容器和展示组件分离)
* stateless function component(react SFC提高性能)
* 扁平访问
* .....

这些都是组件设计的内功心法，如果能讲的比较好的话，面试官应该也会比较开心吧。不过这些并不是教条性质的，具体怎么实现还得结合实际情况。比如，有时候组件粒度不划分到那么小，反而更方便数据管理。在网上搜了一些大神们总结的一些套路，比如阿里大牛写的这几点[react组件设计原则](https://github.com/react-component/react-component.github.io/blob/master/docs/zh-cn/component-design.md),又比如[Make Your React Components Pretty](https://medium.com/walmartlabs/make-your-react-components-pretty-a1ae4ec0f56e)

#### 3.2 招式练习
建议多总结一下平时用到的一些基础组件的实现原理或者设计思路，比如input、textarea、select、modal对话框、级联组件cascader等等，可以多参考学习一下开源方案，比如基于vue的[iview](https://www.iviewui.com/)，前段时间爆出彩蛋事件的[ant design](https://ant.design/)。有时间的话，最好自己去实现一两个玩一玩，动手去写了，也许能有一些不一样的发现。下面是我自己动手实践了一下cascader级联组件，样式直接copy了iview的，逻辑上，并没有像iview那样使用递归，因为递归对于机器来说并不高效，而且不小心容易引起stack overflow（max call stack size exceeded)....

## 四、招式演练（级联组件）
要封装一个组件或者功能模块，比较好的方式是从它的调用方式入手，能想到的最简单的cascader的调用方式大概这样
```js
    <Cascader :list="list" v-model="value" @selectChange="handleChange" />
```
如果想要实现更复杂的功能，可以参考开源方案，比如iview的[级联组件](https://www.iviewui.com/components/cascader)就提供了很多选项,如下图
<img src="/images/cascader-props.png" width="600px" height="600px">
这里实现最基本的功能，主要的逻辑代码总共不超过40行。
模板：
```html
<template>
  <div class="cascader-wrap">
    <div class="input-wrap">
      <input type="text" class="cascader-input" @click="showDrop = !showDrop" :value="res" readonly>
      <i class="icon-close"></i>
    </div>
    <transition name="fade">
      <div class="cascader-dropdown" @click.stop="handleClick" v-show="showDrop">
        <ul v-for="(list,index) in lists" class="cascader-menu">
          <li
            class="cascader-item"
            v-for="item in list"
            :data-id="item.value"
            :data-level="index"
            :key="item.value"
            :class="current[index] && item.value === current[index] ? 'cascader-active' : ''"
          >{{item.label || item.value}}</li>
        </ul>
      </div>
    </transition>
  </div>
</template>
```
逻辑：
```js
export default {
  name: "Cascader",
  data() {
    return {
      showDrop: false,
      res: "",
      current: this.value || [],
      lists: [this.list || []]
    };
  },
  props: ["list", "value"],
  methods: {
    handleClick(e) {
      let data = e.target.dataset,
        level = Number(data.level),
        id = data.id;
      if (this.current[level] !== id) {
        this.current = this.current.slice(0, level);
        this.current.push(id);
        let arr = this.lists[level].filter(item => {
          return item.value === id;
        });
        if (arr[0].children && arr[0].children.length) {
          this.lists = this.lists.slice(0, level + 1);
          this.lists.push(arr[0].children);
        } else {
          //选择最后一列时，确定value以及emit各种事件
          this.res = this.current.slice();
          this.$emit("input", this.current);
          this.showDrop = false;
          this.$emit("on-change", this.current);
        }
      }
    }
  }
}
```
效果大概就如下图这样([在jsfiddle上体验](https://jsfiddle.net/hq120901/6yehmL7r/))：
<img src="/images/cascader.png" width="350px" height="350px">
实际上，很多组件抽出最核心的功能来，码量非常小，但为了业务的需要，往往需要扩充很多功能或者添加各种开关，甚至做一些必要的优化

##  五、小结
组件设计这类问题，既可以很笼统，又可以很具体，在面试那么短的时间里，要回答得很完美并不是那么容易。平时有空的话，还是多动手造一些轮子，或者在面试之前，挑一两个自己比较满意的组件，好好总结总结。