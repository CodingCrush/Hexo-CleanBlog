---
layout:     post
title:      "浅析Python正则表达式中的转义"
tags:       "Python 正则 转义"
date:       2016-10-29 8:00:00
author:     "CodingCrush"
---


# 浅析Python正则表达式中的转义

前几天看到网上有这么一个段子：“这个问题可以用正则表达式解决，那么好吧，现在有两个问题了”
老脸一红，这段子说的不就是我吗。。。
正则表达式仔细地过了两遍，然而看的时候似乎懂了，自己写时却又头脑一片空白。
看来我还没有真的掌握了正则，编程是一门手艺活，纸上得来终觉浅，绝知此事要躬行。
理一理头绪，写成文字，巩固了解。

## Python中字符串中的'\'与前缀r是什么？
在字符串中，\默认是转义符号，它不仅仅是一个字符，而是与跟随的字符共同解释了一个方法。
当字符串前加上r或R，便不需尝试去解释字符串里的方法，默认为原始字符串处理。在SHELL里测试几个例子加深理解。
第一个，r'\"'为了保留'\"'，因为\是一个转义符号，所以\"默认是只是一个"，为了保持\的存在，需要使用\转义\自身。
第二个例子，当\"结合在一起时，转义了自身，因为"是特殊字符，一般是限定一个字符串的区间的。如果\与z 或m 结合在一起时，由于解释器找不到这种定义，便会认为\不是一个转义符号，而仅仅是一个字符，为了保留字符\，那么便需要对字符进行转义，使用\\替换\进行字符保留。
第三个例子：存在\\，它是'\'的转义，因此便不需要对其进行再次转义，因此打印到屏幕上便只剩一个\字符。令人费解的是r'\'应该以 '\\'保存，如第四个一样。但是shell却会报错，暂时搞不明白。
剩下几个例子，很简单，因为shell\n存在转义意义，字符串自身不需要转义即可保存，直到输出时才进行转义解释。


    >>> r'\"'
    '\\"'
    >>> '"'=='\"'
    True  
    
    >>> '\\'
    '\\'
    >>> print('\\')
    \
    
    >>> 'shell\n'
    'shell\n'
    >>> print('shell\ntest')
    shell
    test
    >>> print(r'shell\ntest')
    shell\ntest

## 正则表达式中的转义处理
首先，养成从SHELL中查看官方文档的好习惯，如果还看不明白再进行GOOGLE和STACKOVERFLOW。

    >>> import re
    >>> dir(re)
    ['A', 'ASCII', 'DEBUG', 'DOTALL', 'I', 'IGNORECASE', 'L', 'LOCALE', 'M', 'MULTILINE', 'S', 'Scanner', 'T', 'TEMPLATE', 'U
    ', 'UNICODE', 'VERBOSE', 'X', '_MAXCACHE', '__all__', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__',
    '__name__', '__package__', '__spec__', '__version__', '_alphanum_bytes', '_alphanum_str', '_cache', '_cache_repl', '_comp
    ile', '_compile_repl', '_expand', '_locale', '_pattern_type', '_pickle', '_subx', 'compile', 'copyreg', 'error', 'escape'
    , 'findall', 'finditer', 'fullmatch', 'match', 'purge', 'search', 'split', 'sre_compile', 'sre_parse', 'sub', 'subn', 'sy
    s', 'template']
    >>> print(re.subn.__doc__)
    Return a 2-tuple containing (new_string, number).
        new_string is the string obtained by replacing the leftmost
        non-overlapping occurrences of the pattern in the source
        string by the replacement repl.  number is the number of
        substitutions that were made. repl can be either a string or a
        callable; if a string, backslash escapes in it are processed.
        If it is a callable, it's passed the match object and must
        return a replacement string to be used.

re.subn函数返回了一个2元素元组，由匹配后的字符串与成功匹配数量组成。
定义两个字符串，一个存在转义的字符串，一个原始字符串

    >>> txt='hello\nworld\n!'
    >>> raw_txt=r'hello\nworld\n!'
    >>> raw_txt
    'hello\\nworld\\n!'
    >>> txt
    'hello\nworld\n!'
测试了这个例子，我有点迷惑了，这个原始字符串的r为什么对正则不起作用？明明r'\n'应当是两个字符，却仍然表现为一个换行符进行匹配。也记得书上是说过，Python与正则会对转义进行双重转换，但这个双重转义的影响究竟是如何表现的？再次构造新的例子进行测试分析

    >>> re.subn('\n','A',txt)
    ('helloAworldA!', 2)
    >>> re.subn(r'\n','A',txt)
    ('helloAworldA!', 2)
    >>> re.subn('\n','A',raw_txt)
    ('hello\\nworld\\n!', 0)
    >>> re.subn(r'\n','A',raw_txt)
    ('hello\\nworld\\n!', 0)
从下面这个例子进行双重转义过程分析

    >>> re.subn('\\\\n','A,raw_txt)
    ('helloAworldA!', 2)
    >>> re.subn('\\\n','a',raw_txt)
    ('hello\\nworld\\n!', 0)
转义过程如下：
    
     字符串　　　Python转义　　   正则转义
      '\\\\n'　　　　'\\n'　　   '\n' 换行符
      '\n' 　　　　　'\n'　　　　 '\n' 换行符
      r'\n'　　　　　'\\n'　　　　'\n' 换行符
      '\\\n'　　　　'\\\n'     　'\\\n'