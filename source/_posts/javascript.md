---
title: javascript最佳做法
date: 2020-03-15 12:30:30
tags: 
	- javascript
---
![我爱javascript](https://w.wallhaven.cc/full/76/wallhaven-76opry.png)

写一篇关于最佳做法的文章是一件棘手的事情，对于你们中的一些人来说，你将要读到的内容将非常清晰明了，并且帮助你在实战中作出明智的决定。

然而，围绕互联网上的内容和之前多年查看来自于其他开发者的代码的经历，它教会我一个事实，即现成的来自互联网上的代码，出现常识的缺乏是非常常见的，而且，当你处在某个项目中的时候，明智且合乎逻辑的事情常常被远远搁置在优先级列表中的后面，最后的结果就是无法应对迫在眉睫的截止日期。

因此我决定写这篇文章来让这些事情变得更加简单一点，里面总结了我在这么多年实际工作中掌握的最佳做法以及好的建议，其中很多需要自己实际去尝试才能掌握。

将下面的建议铭记在心并且将它们刻在自己大脑的一部分，这样你才能在需要用到的时候自然而然的想到它们。我确定你会发现一些你不赞同的东西，但是没关系，质疑也是学习的一部分，你应该这样做，努力去寻找你认为更好的解决办法。然而，我发现下面的这些原则让我成为了一个高效的开发者，并且让其他的开发者能够更容易的构建基于我的工作成果的东西。

### 1. 使用名字来调用它们——容易，简短，可读的变量和函数名

这一点是很明确的甚至不需要过多赘述。但是当你在javascrit代码中碰到类似于`x1`，`fe2`或者`xbqne` 这样的变量名，又或者在另一个极端，碰到像`incrementorForMainLoopWhichSpansFromTenToTwenty` 或者`createNewMemberIfAgeOverTwentyOneAndMoonIsFull`这样的变量。

这些变量都没有太大的意义。好的变量和函数名应该很容易理解，并告诉你它将要做的事情，不多不少。需要避免的一个陷阱是：将值和功能结合在一起。一个叫做`isLegalDrinkingAge`的函数比另一个叫做`isOverEighteen`的函数更有意义，因为饮酒年龄在不同的国家可能是不同，而且还要考虑除了饮酒之外也同样受到年龄限制的事情。

匈牙利文符号是可以采取的一个比较好的变量命名方案。它的优点是可以让你知道一个名字的真实含义而不是它看起来的含义。

例如，你有一个叫做`familyName`的变量名，它应该是一个字符串，因此，你可以使用`sFamilyName`来命名它。对于一个叫做member的对象，你可以使用`oMember`来命名它，而对于一个叫做`isLegal`的`Boolean`值，可以使用`bIsLegal`来替代它。这样的命名时非常直观的，对其他人也是一样，是否使用它们完全取决于你。

使用英语命名也是一个好主意，毕竟编程语言本身就是英语。

### 2. 避免使用全局变量

全局变量和函数名绝对是一个非常糟糕的想法。理由是每个被包含在页面中的javascript文件都会在同样的范围内被执行。如果在你的代码中有一个全局变量或者函数，在你之后被引入的其他javascript脚本如果也使用了同样的变量或者函数名，那它们将会覆盖你的变量和函数。

下面列举了几种避免使用全局变量和函数的解决办法：

* 第一种：

```javascript
var myNameSpace = {
    current: null,
    init: function(){
        ...
    },
    change: function(){
        ...
    },
	verify: function(){
        ...
    }
}
```

上面的做法可以解决全局的问题。但是存在一个缺点，即当你每次想要修改变量值或者调用某个函数时，你总是需要通过主对象的名称，比如调用`init()`方法，你需要写成`myNameSpace.init()`，或者调用`current`变量，你需要写成`myNameSpace.current`。这看起来是烦人的也是重复的。

* 第二种：

```javascript
myNameSpace = function(){
    var current = null;
    function init(){
        ...
    }
    function change(){
        ...
    }
    function verify(){
        ...
    }
}();
```

上面的方法将所有内容包裹在一个匿名函数中，并且让执行范围受保护，而且可以使用常规的`function name`方式来声明函数。这种策略叫做模块模式。

但是，这种方式也不是没有缺点，即无法从外部访问内部的变量和函数。解决办法参考第三种。

* 第三种：

```javascript
myNameSpace = function(){
    var current = null;
    function verify(){
        ...
    }
    return {
        init: function(){
            ...
        }
        change: function(){
            
        }
    }
}();
```

这样做可以解决从外部访问内部函数的问题，但是不够优雅。可以使用第四种方法进行改良。

* 第四种：

```javascript
myNameSpace = function() {
	var current = null;
	function init() {
		…
	}
	function change() {
		…
	}
	function verify() {
		…
	}
	return{
		init:init,
		change:change
	}
}();
```

这种方法没有将函数体放在return语句中，而是返回了指向它们的指针。

* 第五种：

```javascript
(function() {
	var current = null;
	function init() {
	    console.log('this is another init method');
	}
	function change() {
        console.log('this is another change method');
	}
	function verify() {
        console.log('this is another verify method');
	}
})();
```

这种方法将所有函数和变量包含在一个小范围内。而且无法从外部访问，但是很容易从在内部共享。

### 3. 使用strict模式

浏览器默认会以兼容模式执行javascript代码，但是为了写出高质量的代码，我们应该以更加严格的方式写js代码。

### 4. 注释不多不少

参考下面这种注释模式：

```javascript
module = function() {
	var current = null;
	function init() {
		…
	};
/*
	function show() {
		current = 1;
	};
	function hide() {
		show();
	};
// */
	return{
		init:init,
		show:show,
		current:current
	}
}();
```

使用多行注释方式时，最好在后面的`*/`前面加上一个单行注释，这样，当我们想要解除多行注释时就只需要在前面的`/*`之前加一个`/`就行了。

### 5. 如果能一行代码解决问题，就不要拆分成多行

```javascript
var f = document.getElementById('mainform');
var inputs = f.getElementsByTagName('input');
for(var i = 0, j = inputs.length; i < j; i++) {
	if (inputs[i].className === 'mandatory' && inputs[i].value === '') {
		inputs[i].style.borderColor = '#f00';
		inputs[i].style.borderStyle = 'solid';
		inputs[i].style.borderWidth = '1px';
	}
}
```

上面的代码中，if语句内部的3行代码最终的目的是给输入框加一个输入错误的边框效果。但却对dom进行了3次操作。这样明显是存在效率问题的。可以采用下面的方法进行改良。

```javascript
var f = document.getElementById('mainform');
var inputs = f.getElementsByTagName('input');
for(var i = 0, j = inputs.length; i < j; i++) {
	if (inputs[i].className === 'mandatory' && inputs[i].value === '') {
		inputs[i].className += ' error';
	}
}
```

只通过添加一个`error`的`css`类即可。这样做更加高效，且更加有利于维护。

### 6. 使用有意义的简写符号

例如我们声明一个对象时，比较传统的做法是这样的：

```javascript
var cow = new Object();
cow.colour = 'brown';
cow.commonQuestion = 'What now?';
cow.moo = function() {
	console.log('moo');
}
cow.feet = 4;
cow.accordingToLarson = 'will take over the world';
```

​	然而，这种做法是冗余的，而且一点也不简洁。可以使用下面的做法来代替。

```javascript
var cow = {
	colour:'brown',
	commonQuestion:'What now?',
	moo:function() {
		console.log('moo');
	},
	feet:4,
	accordingToLarson:'will take over the world'
};
```

对于数组而言，传统的方式是这样的：

```javascript
var aweSomeBands = new Array();
aweSomeBands[0] = 'Bad Religion';
aweSomeBands[1] = 'Dropkick Murphys';
aweSomeBands[2] = 'Flogging Molly';
aweSomeBands[3] = 'Red Hot Chili Peppers';
aweSomeBands[4] = 'Pornophonique';
```

同样存在大量的冗余，而且不利于理解，可以采用下面的方式：

```javascript
var aweSomeBands = [
	'Bad Religion',
	'Dropkick Murphys',
	'Flogging Molly',
	'Red Hot Chili Peppers',
	'Pornophonique'
];
```

这种方式可以让我们更加关注内容而不是它们的index。

三元表示法也可以让我们的代码更加简洁：

```javascript
var direction = (x > 100) ? 1 : -1;
```

对于可能为空的元素，可以通过下面的方法来给定默认值：

```javascript
var x = v || 10;
```

避免使用大量的`if else`块让代码看起来不是那么简洁。

### 7. 模块化代码、一个任务对应一个函数

这也是一个非常好的编程习惯，可以让我们的代码的可读性更高。

### 8. 逐步增强

这个概念的本质是只写能够生效的代码，而忽视技术带来的便利。对于javascript来说，由于用户的浏览器环境各不相同，所有对于可能存在javascript被禁用的情况，这个时候，如何保证你的网站能够正常工作并应对用户的要求就显得尤为重要。

### 9. 允许配置和翻译

为了保证代码的高可用性和可维护性，实现可配置编码是必须的。

### 10. 避免深度嵌套

提取公共代码是解决深度嵌套的理想解决办法。

### 11. 优化循环

优化下面的代码：

```javascript
var names = ['George', 'Ringo', 'Paul', 'John'];
for(var i = 0; i < names.length; i++) {
	doSomeThingWith(names[i]);
}
```

可以看出，循环判断条件中使用了`names.length`作为判断依据，但是，通常情况下，`names.length`的值是一个定值，我们完全可以将它事先保存在一个变量中，这样就可以避免每次循环都要从`names`对象中提取`length`属性的值。

```javascript
var names = ['George', 'Ringo', 'Paul', 'John'];
var all = names.length;
for(var i = 0; i < all; i++) {
	doSomeThingWith(names[i]);
}
```

如果我们不想单独定义一个外部变量，也可以这样做：

```javascript
var names = ['George', 'Ringo', 'Paul', 'John'];
for(var i = 0, j = names.length; i < j; i++) {
	doSomeThingWith(names[i]);
}
```

另外需要注意的一点是：避免将计算量较大的代码放在循环里面，其中包括正则表达式的判定和DOM元素的操作。你可以在循环中创建DOM节点，但是不要将它们插入到DOM中。

### 12. 将DOM操作最小化

在浏览器中访问DOM的代价是非常昂贵的。它的渲染非常耗费时间。因此，除非必要，尽量避免在javascript中对DOM进行太多的操作。

### 13. 不要屈服于某个浏览器的某些特性

我们应该遵从业界标准去开发通用代码，而不应该仅仅局限于某个厂商的浏览器，尽管它的市场份额可能比较可观。

如果你必须使用某个厂商的浏览器的某个特性，尽量将使用了该特性的代码进行标记，方便日后的维护。

### 14. 不要相信任何数据

不仅为了应付可能存在的恶性攻击，同时也是为了保证代码的健壮性，避免因为用户的错误输入内容导致系统出错。

当我们处理用户输入的数据时，必须确保该数据类型是我们所期望的。避免出现类型错误。下面的代码展示了怎样确认一个数组参数：

```javascript
function buildMemberList(members) {
	if (typeof members === 'object' &&
		typeof members.slice === 'function') {
		var all = members.length;
		var ul = document.createElement('ul');
		for(var i = 0; i < all; i++) {
			var li = document.createElement('li');
			li.appendChild(document.createTextNode(members[i].name));
			ul.appendChild(li);
		}
		return ul;
	}
}
```

因为数组本身也是对象，但是，我们不能简单的认为一个对象就一定是数组，最好的验证办法是检验数组的某个特征函数。比如上面就用到了`slice`函数来区分一般对象和数组对象。

### 15. 不要在javascript中添加太多的html内容。

对于变化频繁的HTML内容，使用模板技术是一个不错的选择，可以避免频繁修改javascript代码。

```javascript
var playercontainer = document.getElementById('easyyoutubeplayer');
if (playercontainer) {
	ajax('template.html');
};

function ajax(url) {
	var request;
	try {
		request = new XMLHttpRequest();
	} catch(error) {
		try {
			request = new ActiveXObject('Microsoft.XMLHTTP');
		} catch(error) {
			return true;
		}
	}
	request.open('get', url, true);
	request.onreadystatechange = function() {
		if (request.readyState == 4) {
			if (request.status) {
				if (request.status === 200 || request.status === 304) {
					if (url === 'template.html') {
						setupPlayer(request.responseText);
					}
				}
			} else {
				alert('Error: Could not find template…');
			}
		}
	};
	request.setRequestHeader('If-Modified-Since','Wed, 05 Apr 2006 00:00:00 GMT');
	request.send(null);
};
```

上面的代码展示了一个完整的异步请求数据模板的过程。避免了直接在javascript中编写大量HTML代码的问题。

### 16. 站在巨人的肩膀上构建

随着越来越多的javascript库的出现，构建网站的代价也变得没有那么高了。如何有效利用这些库对于一个高效的开发人员来说尤为重要。

下面列出一些优秀的javascript框架：

* jquery：[jquery插件库](https://www.jqueryscript.net/)
* underscorejs：[函数式编程库](https://underscorejs.bootcss.com/)

### 17. 开发中的代码并不代表线上代码

如果我们专注于编写更加浅显易懂且容易被其他程序员进行拓展的代码，那么我们同时也可以编写出完美的构建脚本，如果我们过早的进行优化，我们将永远无法实现目标。不要为你自己或者浏览器构建，而要为下一个接替你的开发者来构建。

## 总结

编写javascript代码的主要技巧就是避免走捷径。不要轻易陷入这门语言的语法陷阱，否则，它们迟早会让你陷入麻烦。