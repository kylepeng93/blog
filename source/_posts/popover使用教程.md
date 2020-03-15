## bootstrap popover使用教程

使用popover插件前需要知道的几件事情:

* Popover出于性能考虑是需要手动配置的，因此你必须自己初始化它的属性。
* 长度为0的title和content不会被展示。
* 当在复杂组件中使用的时候，指定`container: 'body'`可以避免出现渲染问题。比如输入组或者按钮组。
* 在隐藏的元素上无法触发popover。
* 对于使用了`disabled`或者`.disabled`的元素，必须使用包装器才能触发。
* 对于跨越了多行的超链接元素，popover被触发时将会居中显示。可以对`<a>`使用`white-space: nowrap`来避免这个行为。
* 如果响应元素被从DOM中移除，那么popover一定会被隐藏。

### 示例1：启用popover

可以通过给元素设置data-toggle属性来初始化页面上的所有的popover元素。

```javascript
$(function () {
    $('[data-toggle="popover"]').popover();
})
```

### 示例2： 使用`container`选项

当你在父元素中定义了一些样式，并且想应用在popover上时，你需要指定一个自定义`container`来告诉popover将它的HTML内容包裹在你指定的父元素里面。

```javascript
$(function () {
    $('.example-popover').popover({
        container: 'body'
    })
})
```

### 示例3： 使用`placement`选项

placement有5个值：top，right，bottom，left，分别代表上、右、下、左四个方向。

```javascript
$(function () {
    $('.example-popover').popover({
        placement: 'top'
    })
})
```



### 示例4：使用`trigger`选项

```javascript
$(function () {
    $('.example-popover').popover({
        trigger: 'focus'
    })
})
```

trigger有四个选项值，分别时hover、focus、click、manual。

其中manual为手动触发，可以自己编写触发逻辑。比如：

```javascript
$(function () {
    $('.example-popover').popover({
        trigger: 'manual'
    }).on("mouseover", function () {
        // 编写鼠标进入时的逻辑
        var _self = this;
        // 清空计时器
        clearTimeout(counter);
        counter = setTimeout(function(){
            // 当鼠标放到popover启动元素上面时
            if($(_self).is(':hover')){
                $(_self).popover('show');
            }
            $('.popover').on("mouseleave", function(){
                $(_self).popover('hide');
            })
        },400);
    }).on("mouseleave", function () {
        // 编写鼠标离开时的逻辑
        var _self = this;
        if(!$('.popover:hover').length) {
            // 当鼠标不在popover窗口上方的时候
            if(!$(_self).is(':hover').length) {
                // 当鼠标同时也不在popover触发元素上方时
                $(_self).popover('hide');
            }
        }
    })
})
```

> 使用`focus`作为`trigger`时，为了保证功能的跨平台性，必须使用`<a>`作为触发元素。并且使用tabinex属性。

### 示例5：使用了`disabled`的元素

对于使用了`disabled`属性的元素是无法产生交互的，意味着用户无法`hover`或者`click`它们，也就无法触发popover（tooltip）。作为解决办法之一，你可以使用`<div>`或者`<span>`标签来包装这个元素，并且覆写被禁用元素的`<point-events>`样式来触发popover。

```html
<span class="d-inline-block" data-toggle="popover" data-content="Disabled	popover">
	<button class="btn btn-primary" style="pointer-events: none;" type="button" disabled>
        Disabled button</button>
    </button>
</span>
```

### 示例6：带图片的popover

```html
<a class="btn btn-primary" data-toggle="popover-hover" data-img="https://mdbootstrap.com/img/logo/mdb192x192.jpg">Hover
  me</a>
```

## popover选项的使用说明：

| 选项名       | 类型                      | 默认值                                                       | 描述                                                         |
| ------------ | ------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| animation    | boolean                   | true                                                         | 添加一个淡入淡出的效果                                       |
| boundary     | String\|element           | 'scrollParent'                                               | popover的溢出约束边界，可以接受`viewport`，`window`，`scrollParent`，或者HTML元素引用。 |
| container    | String\|false             | false                                                        | 将popover追加到一个指定的元素，比如：`container: 'body'`。这个选项非常有用，它允许你将popover放置在靠近触发元素的文档流中，这样可以阻止popover在窗口缩放的时候因为浮动而远离触发元素。 |
| content      | String\|element\|function | ''                                                           | 作为默认的展示内容，如果没有使用data-content属性的话。如果指定了一个function，只有this引用被指向绑定了popover标识的元素时才会被调用。 |
| contstraints | Array                     | []                                                           | 一个约束数组，被传递给Tether。                               |
| delay        | number\|object            | 0                                                            | 设置popover展示和隐藏时的延迟，不适用于`manual`类型。设置格式为：`delay: {"show": 500, "hide": 100}` |
| html         | boolean                   | false                                                        | 向popover中插入HTML标签。如果是false，jQuery的text方法会被用来向DOM中插入内容。如果你担心XSS攻击的话最好使用text。 |
| placement    | String\|function          | 'top'                                                        | 用来指定popover的位置。可以使用的值有以下几个：auto：动态的重定位popover。当指定一个function来决定方向时，需要将它作为第一个参数，而trigger作为第二个参数。`this`将被指向popover实例。 |
| selector     | string                    | false                                                        | 如果一个selector被提供，popover对象将会被委托给指定的目标。实际上，它通常会被用来向popover中插入动态HTML内容。 |
| template     | String                    | '<div class="popover" role="popover"><div class="popover-arrow"></div><div class="popover-inner"></div></div>' | 创建popover的基础HTML模板，popover的title将会被注入到`.popover-header`。popover的content将会被注入到`.popover-body`。.arrow将会成为popover的箭头。最外面的包装器元素应该含有.popover类。 |
| title        | string\|element\|function | ''                                                           | 作为默认值，如果title属性没有被声明的话。如果指定了一个function，当this引用被指向popover绑定的元素时，该函数才会被调用。 |
| trigger      | string                    | 'hover focus'                                                | 指定popover的触发方式。可以指定为以下几个值 - click\|hover\|focus\|manual。你可以传递多个触发器。并用空格区分，manual属性值不能和其他值一起使用。 |
| offsets      | string                    | '0 0'                                                        | 相对于它的目标的偏移量。                                     |

## popover中可以使用的方法

* .popover('show')

  用来显示一个popover

* .popover('hide')

  用来隐藏一个popover。        

* .popover('toggle')

  隐藏/显示一个popover

* .popover('dispose')

  隐藏并销毁一个popover

* .popover('enable')

  启用一个popover

* .popover('disable')

  禁用一个popover

* .popover('toggleEnabled')

  启用/禁用popover的显示和隐藏的能力

* .popover('update')

  更新一个元素的popover的位置

## popover中可以使用的事件

| 事件类型            | 描述                                                        |
| ------------------- | ----------------------------------------------------------- |
| show.bs.popover     | 当show方法被调用时触发                                      |
| shown.bs.popover    | 当popover对用户可见时触发                                   |
| hide.bs.popover     | 当hide方法被调用时触发                                      |
| hidden.bs.popover   | 当popover对用户完全不可见触发                               |
| inserted.bs.popover | 在show.bs.popover事件之后触发，且popover模板被插入到DOM中。 |

```javascript
$("#myPopover").on("hidden.bs.popover", function () {
    // do something
})
```
