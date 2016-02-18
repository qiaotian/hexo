title: Unix Shell Programming
date: 2016-1=02-14
tag: shell
---

## grep (globally search a regular expression and print)
>grep is a command-line utility for searching plain-text data sets for lines matching a regular expression. Grep was originally developed for the Unix operating system, but is available today for all Unix-like systems.

A simple example of a common usage of grep is the following, which searches the file fruitlist.txt for lines containing the text string apple:
```bash
$ grep apple file.txt
```
Multiple file names may be specified in the argument list. For example, all files having the extension .txt in a given directory may be searched if the shell supports globbing by using an asterisk as part of the filename:
```bash
$ grep apple *.txt
```
Regular expressions can be used to match more complicated text patterns. The following prints all lines in the file that begin with the letter a, followed by any one character, followed by the letter sequence ple.
```bash
$ grep ^a.ple file.txt
```

## head
>head 用来显示档案的开头至标准输出中，默认head命令打印其相应文件的开头10行。
命令格式
```bash
head [-opt] ... [filename] ...
```
示例如下
```bash
head -n  5 file.txt //显示file.txt的前5行
head -n -5 file.txt //显示file.txt的除最后5行以外的所有行
head -c  5 file.txt //显示file.txt的前5个字节
head -c -5 file.txt //显示file.txt的除最后5个字节外的所有字节
```

## tail
命令格式
```bash
tail [-opt] ... [filename] ...
```
```bash
tail -n  5 file.txt //显示file.txt的最后5行
tail -n +5 file.txt //显示file.txt从第5行开始的所有内容
tail -f    file.txt //刷新显示file.txt最后一行内容
```

## sed

## awk
AWK是三位创始人 Alfred Aho，Peter Weinberger, 和 Brian Kernighan 的Family Name的首字符，命令格式：
```bash
awk '{awk命令}' filename
```
## nawk (The new awk)

## gawk (GNU awk)


## **REFERENCE**
https://en.wikipedia.org/wiki/Grep
http://coolshell.cn/articles/9070.html
[GNU Awk User Guide](http://www.gnu.org/software/gawk/manual/gawk.html)>
http://www.cnblogs.com/peida/archive/2012/11/06/2756278.html
