---
title: Python Mixin机制
date: 2019-08-15 15:31:57
tags: [python]
---

在面向对象编程中，Mixin是一种类，这种类包含了其他类要使用的方法，但不必充当其他类的父类。
其他类是如何获取Mixin中的方法因语言的不同而不同。所以有时候Mixin被描述为`include`（包含）而不是`inheritance`（继承）。

Mixins鼓励代码重用，并且可以用于避免多重继承可能导致（如钻石问题）的继承歧义，或者解决一个缺乏对一种语言的多重继承的支持。
Mixin也可以被看作`实现方法`的接口。这种模式是强制依赖性反转原理的一个例子。

Mixins是一种语言概念，允许程序员将一些代码注入到一个类中。
Mixin编程是一种软件开发的风格，其中功能单元在一个类中创建，然后与其他类混合。
Mixin类扮演父类的角色，包含其他类想要的功能。

<!--more-->

## 一个简单的例子

在自然界中，有些鸟会飞、有些不会，还有些鸟会游泳。这时候我们可以将飞行、游泳这种能力设计成mixins，代码如下：

```python
class Animal(object):
    pass

class Bird(Animal):
    """鸟类"""
    pass

class SwimMixin(object):
    def swim(self):
        print('can swim')

class FlyMixin(object):
    def fly(self):
        print('can fly')

class Penguin(Bird, SwimMixin):
    """企鹅"""
    pass

class Eagle(Bird, FlyMixin):
    """鹰"""
    pass

class Ostrich(Bird):
    """鸵鸟"""
    pass
```

当然，有些哺乳动物也会飞行：

```python
class Mammal(Animal):
    """哺乳动物"""
    pass

class Bat(Mammal, FlyMixin):
    """蝙蝠"""
    pass
```

## 在Django项目中的应用

Django项目中大量使用了Mixins方式编程，如`middleware`、`forms`、`views`等。

```python
class ContextMixin:
    """
    A default context mixin that passes the keyword arguments received by
    get_context_data() as the template context.
    """
    extra_context = None

    def get_context_data(self, **kwargs):
        kwargs.setdefault('view', self)
        if self.extra_context is not None:
            kwargs.update(self.extra_context)
        return kwargs

class TemplateResponseMixin:
    """A mixin that can be used to render a template."""
    template_name = None
    template_engine = None
    response_class = TemplateResponse
    content_type = None

    def render_to_response(self, context, **response_kwargs):
        """
        Return a response, using the `response_class` for this view, with a
        template rendered with the given context.
        Pass response_kwargs to the constructor of the response class.
        """
        response_kwargs.setdefault('content_type', self.content_type)
        return self.response_class(
            request=self.request,
            template=self.get_template_names(),
            context=context,
            using=self.template_engine,
            **response_kwargs
        )

    def get_template_names(self):
        """
        Return a list of template names to be used for the request. Must return
        a list. May not be called if render_to_response() is overridden.
        """
        if self.template_name is None:
            raise ImproperlyConfigured(
                "TemplateResponseMixin requires either a definition of "
                "'template_name' or an implementation of 'get_template_names()'")
        else:
            return [self.template_name]

class TemplateView(TemplateResponseMixin, ContextMixin, View):
    """
    Render a template. Pass keyword arguments from the URLconf to the context.
    """
    def get(self, request, *args, **kwargs):
        context = self.get_context_data(**kwargs)
        return self.render_to_response(context)
```
