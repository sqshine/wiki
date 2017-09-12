---
layout: post
title: Netty ByteBuf
date: 2016-11-16 10:20:00
tags:
- Atom
categories: Text Editor
description: The post will introduce a text editor Atom.
---



# ByteBuf
### 获取状态

|          method                      |                      Desc                    |
| ------------------------------------ | -------------------------------------------- |
| ByteBuf markReaderIndex()            | 是否打开                                      |
| ByteBuf resetReaderIndex()           | 是否打开                                      |
| ByteBuf markWriterIndex()            | 是否打开                                      |
| ByteBuf resetWriterIndex()           | 是否打开                                      |



ByteBuf的读写都有顺序读和顺序写。
* readXXX：顺序读
* getXXX:随机读
* writeXXX:顺序写
* setXXX:随机写
顺序读和写，index是会变得。随机读和写，index不变

|          method                          |                      Desc                                                |
| ---------------------------------------- | ------------------------------------------------------------------------ |
| boolean readBoolean()                    | 从当前readerIndex读取一个字节，返回一个boolean。readerIndex会加1               |
| byte readByte()                          | 从当前readerIndex读取一个字节，返回一个byte。readerIndex会加1                  |
| int readInt()                            | 从当前readerIndex获取一个32位的int，readerIndex会加4                         |
| boolean getBoolean(int index)            | 从index获取一个1位的boolean，readerIndex不变                                |
| byte getByte(int index)                  | 从index获取一个1位的byte，readerIndex不变                                   |
| int getInt(int index)                    | 从index获取一个4位的int，readerIndex不变                                   |
| ByteBuf setInt(int index,int value)      | 在index写入一个4位的int，writerIndex不变                                   |


```text
0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0
```

buf.writeBytes("how i need you!".getBytes())

```text
104 | 111 | 119 | 32 | 105 | 32 |110 | 101 | 101 | 100 | 32 | 121 | 111 | 117 | 33 | 0 | 0 | 0 | 0 
h   | o   | w   |    | i   |    |n   | e   | e   | d   | 32 | y   | o   | u   | !  | 0 | 0 | 0 | 0 
```


buf.setInt(3,12)
```text
0 | 0 | 0 | 0 | 0 | 0 | 12 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0
```
buf.setByte(5,12);
```text
0 | 0 | 0 | 0 | 0 | 12 | 12 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0
```
