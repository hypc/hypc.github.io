---
title: JavaScript：模仿接口
date: 2018-09-26 09:46:40
tags: [javascript]
---

JavaScript中模仿接口的三种方法：注释法、属性检查法和鸭式辩型法。没有哪种技术是完整的，但三者结合使用基本上可以令人满意。

## 用注释描述接口

用注释模仿接口是最简单的方法，但效果却是最差的。
这种方法模仿其他面向对象语言中的做法，使用了`interface`和`implements`关键字，
但把它们放在注释中，以免引起语法错误。下面的示例演示了如何把这些关键字添加到代码中以描述所要求的方法：

```javascript
/*
    interface Composite {
        function add(child);
        function remove(child);
        function getChild(index);
    }

    interface FormItem {
        function save();
    }
*/

// implements Composite, FormItem
var CompositeForm = function(id, method, action) {
    ...
};

// Implement the Composite interface.

CompositeForm.prototype.add = function(child) {
    ...
};
CompositeForm.prototype.remove = function(child) {
    ...
};
CompositeForm.prototype.getChild = function(index) {
    ...
};

// Implement the FormItem interface.

CompositeForm.prototype.save = function() {
    ...
};
```

这种模仿并不是很好。它没有为确保CompositeForm真正实现了正确的方法集而进行检查，也不会抛出错误以告知程序员程序中有错误。
说到底它只要还是属于程序文档范畴。在这种做法中，对接口约定的遵守完全依靠自觉。

尽管如此。这种方法也有其优点。它易于实现，不需要额外的类或函数。
它可以提高代码的课重用性，因为现在哪些类实现的接口都有说明，程序员可以把它们与实现了同样接口的类互换使用。
这种方法并不影响文件尺寸或执行速度，因为它所使用的注释可以在对代码进行部署时不费吹灰之力地予以剔除。
但是，由于不会提供错误信息，它对测试和调试没有什么帮助。

<!--more-->

## 用属性检查模仿接口

第二种方法更严谨一点。所有类都明确地声明自己实现了那些接口，那些想与这些类打交道的对象可以针对这些声明进行检查。
那些接口自身仍然只是注释，但现在你可以通过检查一个属性得知某个自称实现了什么借口：

```javascript
/*
    interface Composite {
        function add(child);
        function remove(child);
        function getChild(index);
    }

    interface FormItem {
        function save();
    }
*/

var CompositeForm = function(id, method, action) {
    this.implementsInterfaces = ['Composite', 'FormItem'];
    ...
};

...

function addForm(formInstance) {
    if(!implements(formInstance, 'Composite', 'FormItem') {
        throw new Error("Object does not implement a required interface.");
    }
    ...
}

// The implements function, which checks to see if an object declares
// that itimplaments the required interfaces.

function implements(object) {
    // Looping through all arguments after the first one.
    for(var i = 1; i < arguments.length; i++) {
        var interfaceName = arguments[i];
        var interfaceFound = false;
        for(var j = 0; j < object.implementsInterfaces.length; j++) {
            if(object.implementsInterfaces[j] === interfaceName) {
                interfaceFound = true;
                break;
            }
        }
        if(!interfaceFound) {
            return false; // An interface was not found.
        }
    }
}
```

在这个例子中，CompositeForm宣称自己实现了Composite和FormItem这两个接口，
其做法是把这两个接口的名称加入一个名为implementsInterfaces的数组。类显式声明自己支持什么接口。
任何一个要求其参数属于特定类型的函数都可以对这个属性进行检查，并在所需要接口未在声明之列时抛出一个错误。

这种方法有几个优点。它对类所实现的接口提供了文档说明。
如果需要的接口不在一个类宣称的接口之列，你会看到错误消息。通过利用这些错误，你可以强迫其他程序员声明这些接口。

这种方法的主要缺点在于它并非确保类真正实现了自称实现的接口。你只知道它是否说自己实现了接口。
在创建一个类时声明它实现了一个接口，但后来在实现该接口所规定的方法时却漏掉其中某一个，这一种错误很常见。
此时所有类型检查都能通过，但那个方法却并不存在，这将在代码中埋下一个隐患。另外，显式声明类所支持的接口也需要一些额外工作。

## 用鸭式辩型法模仿接口

其实，类是否声明自己支持哪些接口并不重要，只要它具有这些接口中的方法就行。
鸭式辩型（这个名称来自James Whitecomb Riley的名言：“像鸭子一样走里并且嘎嘎叫的就是鸭子”）正是基于这样的认识。
它把对象实现的方法集作为判断它是不是某个类的实例的唯一标准。这种技术在检查一个类是否实现了某个接口时也可大显身手。
这种方法背后的观点很简单：如果对象具有与接口定义的方法同名的所以方法，那么就可以认为它实现了这个接口。你可以用一个辅助函数来确保对象具有所以必需的方法：

```javascript
// Interfaces

var Composite = new Interface('Composite', ['add', 'remove', 'getChild']);
var FormItem = new Interface('FormItem', ['save']);

// CompositeForm class

var CompositeForm = function(id, method, action) {
    ...
};

...

function addForm(formInstance) {
    ensureImplements(formInstance, Composite, FormItem);
    // This function will throw an error if a required method is not implemented.
    ...
}
```

与另外两种方法不同，这种方法并不借助于注释。其各个方面都是可以强制实施的。
ensureImplements函数需要至少两个参数。第一个参数是想要检查的对象。其余参数是据以对那个对象进行检查的接口。
该函数检查其第一个参数代表的对象是否实现了那些接口所声明的所有方法。
如果发现的漏掉了任何一个方法，它就会抛出错误，其中包含了所缺少的那个方法和未被正确实现的接口的名称等有用信息。
这种检查可以用在代码中任何需要确保某个对象实现了某个接口的地方。在本例中，addForm函数仅当一个表单对象支持所有必要的方法时才会对其执行添加操作。

尽管鸭式辩型可能是上述三种方法中最有用的一种，但它也有缺点。在这种方法中，类并不声明自己实现了哪些接口，
这降低了代码的可重用性，并且也缺乏其他两种方法那样的自我描述性。
它需要使用一个辅助类（Interface）和一个辅助函数（ensureImplements）。而且，它只关心方法的名称，并不检查其参数的名称、数目或类型。

## 推荐

一般可综合使用第一种和第三种方法。用注释声明类支持的接口，从而提高代码的可读性及其文档的完整性。
用辅助类`Interface`及其类方法`Interface.ensureImplaments`来对对象实现的方法进行显式检查，如果对象未能通过检查，这个方法将返回一条有用的消息。

下面是一个Interface类的定义：

```javascript
// Contructor.

var Interface = function(name, methods) {
    if(arguments.length != 2) {
        throw new Error("Interface constructor called with " + arguments.length + " arguments, but expected exactly 2.");
    }

    if(typeof name !== "string") {
        throw new Error("Interface constructor expects name to be passed in as a string.");
    }
    this.name = name;
    this.methods = [];
    for(var i = 0; i < methods.length; i++) {
        if(typeof methods[i] !== "string") {
            throw new Error("Interface constructor expects method names to be passed in as a string.");
        }
        this.methods.push(methods[i]);
    }
}

// Static class method.

Interface.ensureImplements = function(object) {
    if(arguments.length < 2) {
        throw new Error("Function Interface.ensureImplements called with " + arguments.length + " arguments, but expected at least 2.");
    }

    for(var i = 1; i < arguments.length; i++) {
        var interface = arguments[i];
        if(interface.constructor !== Interface) {
            throw new Error("Function Interface.ensureImplements expects arguments two and above to be instances of Interface.");
        }
        for(var j = 0; j < interface.methods.length; j ++) {
            var method = interface.methods[j];
            if(!object[method] || typeof object[method] !== "function") {
                throw new Error("Function Interface.ensureImplements: object does not implement the " + interface.name + " interface. Method " + method + " was not found.");
            }
        }
    }
}
```

从中可以看到，该类的所有方法对其参数都有严格的要求，如果参数未能通过检查，将导致错误抛出。
我们特地加入这种检查的目的在于：如果没有错误被抛出，那么你可以肯定接口已经得到了正确的声明和实现。
