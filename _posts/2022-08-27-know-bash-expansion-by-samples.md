---
layout: post
title: "通过例子认识bash扩展"
categories: linux bash
tags: samples
---
## 例子
### 显示test目录下，除build.sh和build.cfg外的所有文件
```
code@code:~/test$ echo *
16M.pdf 50M.pdf build.cfg build.sh
code@code:~/test$ echo !(build.sh|build.cfg)
16M.pdf 50M.pdf
```

参考如下：使用!(pattern-list)方式，用\|做为分割符，排除(except)特定的值

```
If the extglob shell option is enabled using the shopt builtin, 
several extended pattern matching operators are recognized. 
In the following description, a pattern-list is a list of one or more patterns 
separated by a ‘|’. 
Composite patterns may be formed using one or more of the following sub-patterns:

?(pattern-list)
Matches zero or one occurrence of the given patterns.

*(pattern-list)
Matches zero or more occurrences of the given patterns.

+(pattern-list)
Matches one or more occurrences of the given patterns.

@(pattern-list)
Matches one of the given patterns.

!(pattern-list)
Matches anything except one of the given patterns.
```

### 显示变量名和变量的值
```
code@code:~/test$ env | grep TERM
COLORTERM=truecolor
TERM_PROGRAM=vscode
TERM_PROGRAM_VERSION=1.70.1
TERM=xterm-256color

code@code:~/test$ 
for varName in COLORTERM TERM_PROGRAM TERM_PROGRAM_VERSION TERM; 
    do     echo "$varName:${!varName}"; 
done
COLORTERM:truecolor
TERM_PROGRAM:vscode
TERM_PROGRAM_VERSION:1.70.1
TERM:xterm-256color
```
参考使用${!name}使用间接引用(indirect expansion)的方式, 使用变量值
```
If the first character of parameter is an exclamation point (!), and parameter is not a nameref, it introduces a level of indirection. Bash uses the value formed by expanding the rest of parameter as the new parameter; 
```
## 参考
https://www.gnu.org/software/bash/manual/bash.html