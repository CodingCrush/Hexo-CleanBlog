---
layout:     post
title:      Python functools库中wraps装饰器的作用
tags:       [Python,Library,Syntax]
date:       2016-10-23 8:00:00
author:     "CodingCrush"
---

## 使用@wraps装饰器

在Python的使用中，常见到wraps装饰器的出现，由于对其作用不太明白，便通过阅读源代码进行深入了解。
  
    def email_check(email_address):
        def decorator(f):
            @wraps(f)
            def decorated_function(*args, **kwargs):
                if not current_user.email == email_address:
                    abort(403)
                return f(*args, **kwargs)
            return decorated_function
        return decorator


## wraps函数

通过PyCharm 这一IDE提供的强大功能追溯wraps到functools库中，分析wraps构造可知：装饰器本质是一个函数，@仅仅是调用这个函数的一个符号。wraps函数有3个入参，分别为被包装的wrapped函数，assigned与updated，如果不指定需要更换的属性，那么wraps便只使用默认的WRAPPER_ASSIGNMENTS和WRAPPER_UPDATES，其返回的是一个partial偏函数.

    def wraps(wrapped,
              assigned = WRAPPER_ASSIGNMENTS,
              updated = WRAPPER_UPDATES):
        """Decorator factory to apply update_wrapper() to a wrapper function
    
           Returns a decorator that invokes update_wrapper() with the decorated
           function as the wrapper argument and the arguments to wraps() as the
           remaining arguments. Default arguments are as for update_wrapper().
           This is a convenience function to simplify applying partial() to
           update_wrapper().
        """
        return partial(update_wrapper, wrapped=wrapped,
                       assigned=assigned, updated=updated)
                       
### partial函数的作用

查阅官方文档,得到一个代码示例，partial通过改写int函数的base参数为2，将int转为binary函数，每次使用Int时，不再需要分别设置base参数，简化了函数复用的复杂度。

    >>> from functools import partial
    >>> basetwo = partial(int, base=2)
    >>> basetwo.__doc__ = 'Convert base 2 string to an int.'
    >>> basetwo('10010')
    18
    
## wraps装饰器返回的partial函数
    
>* update_wrapper(包装处理函数）
>* wrapped=wrapped（被包装的函数）
>* assigned=assigned（assigned属性，默认为WRAPPER_ASSIGNMENTS入参）
>* updated=updated（updated属性，默认为WRAPPER_UPDATES入参）

### 属性元组

    WRAPPER_ASSIGNMENTS = ('__module__', '__name__', '__qualname__', '__doc__',
                           '__annotations__')
    WRAPPER_UPDATES = ('__dict__',)

    
## update_wrapper函数的包装过程

    def update_wrapper(wrapper,
                       wrapped,
                       assigned = WRAPPER_ASSIGNMENTS,
                       updated = WRAPPER_UPDATES):
        """Update a wrapper function to look like the wrapped function
    
           wrapper is the function to be updated
           wrapped is the original function
           assigned is a tuple naming the attributes assigned directly
           from the wrapped function to the wrapper function (defaults to
           functools.WRAPPER_ASSIGNMENTS)
           updated is a tuple naming the attributes of the wrapper that
           are updated with the corresponding attribute from the wrapped
           function (defaults to functools.WRAPPER_UPDATES)
        """
        for attr in assigned:
            try:
                value = getattr(wrapped, attr)
            except AttributeError:
                pass
            else:
                setattr(wrapper, attr, value)
        for attr in updated:
            getattr(wrapper, attr).update(getattr(wrapped, attr, {}))
        # Issue #17482: set __wrapped__ last so we don't inadvertently copy it
        # from the wrapped function when updating __dict__
        wrapper.__wrapped__ = wrapped
        # Return the wrapper so this can be used as a decorator via partial()
        return wrapper
    
看到这里，wraps装饰器的作用已经解析的八九不离十了，updated_wrapper函数有3个过程：
> 1. 对于`('__module__', '__name__', '__qualname__', '__doc__', '__annotations__')` 这个元组的里属性，由于它是函数相关的，不同的函数对象通常有不同的名字，不同的模块与不同的注释和说明，而装饰器虽然装饰了一个函数，看似功能与原函数基本一致志，然而确如同克隆人是两个完全不同的函数，因此它们的这些属性是不同。代码中通过try，except分别尝试获取wrapped对象的这些属性，如果存在，则将这些属性绑定到全新的wrapper对象上。getattr获取的则是对象属性的一个内存地址，类似C语言中的指针。分析setattr可知，给对象增加方法本质上是增加一个指向方法的指针。
> 2. `__dict__`字典中存储的是wrapped对象的属性，通过迭代器分别将其传递给wrapper函数，以实现对wrapped函数部分属性的复制。
> 3. `wrapper.__wrapped__ = wrapped`将被包装函数写为wrapper的方法。

## wraps函数的柯里化

从官方文档中，查阅到wraps实现中，与文首的装饰器相比少了一层嵌套

    def my_decorator(f):
    ...     @wraps(f)
    ...     def wrapper(*args, **kwds):
    ...         print 'Calling decorated function'
    ...         return f(*args, **kwds)
    ...     return wrapper
    
本文首的代码块实际上是这样调用email_check函数的:

    def admin_required(f):
        return email_check(Config.FLASK_ADMIN_EMAIL)(f)

>[柯里化][1]，是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术。

文首代码中嵌套的decorator(f)函数的作用是为了分层取得参数，email_check拿到第一个括号内的参数，而decorator则拿到第二个入参，如果去掉decorator，函数改造成这样也是错误的，因为*args,  **kwargs 对应的是f()的传参，而email_check入参却不仅仅只是一个f函数对象：

        def email_check(email_address, f):
            @wraps(f)
            def decorated_function(*args, **kwargs):
            if not current_user.email == email_address:
                    abort(403)
                return f(*args, **kwargs)
            return decorated_function

同理调用则需要改成这样：

    def admin_required(f):
        return email_check(Config.FLASK_ADMIN_EMAIL, f)

  [1]: https://zh.wikipedia.org/wiki/%E6%9F%AF%E9%87%8C%E5%8C%96