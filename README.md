# consoleLogMemory
## 如何使用
```
npm insatll / yarn instsall  / pnpm install 

node server
```
## 结论
**当控制台打开时** console.log( x ) 导致对象 x 始终被引用，无法被垃圾回收，造成内存泄漏

**当控制台关闭时** console.log 不会有内存泄漏的问题
> 不开的时候，devtools frontend没有连接，此时console.log 的信息不会真正发送，而是进入到V8ConsoleMessageStorage里面暂存，而这个暂存池设置了1000条/10MB的上限，超了的话会吐掉最早的信息，因此不会内存爆炸。不开的时候，devtools frontend没有连接，此时console.log 的信息不会真正发送，而是进入到V8ConsoleMessageStorage里面暂存，而这个暂存池设置了1000条/10MB的上限，超了的话会吐掉最早的信息，因此不会内存爆炸。详见 v8/src/inspector/v8-console-message.cc 里555行 addMessage 的实现

具体解析见作者参考的三篇文章
- [千万别让 console.log 上生产！用 Performance 和 Memory 告诉你为什么](https://juejin.cn/post/7185128318235541563#comment)
- [console.log 一定会导致内存泄漏？不打开 devtools 就不会](https://juejin.cn/post/7185501830040944698#comment)
- [Can console.log() cause memory leaks? How to make a browser crash with console.log()?](https://javascript.plainenglish.io/can-console-log-cause-memory-leaks-how-to-make-a-browser-crash-with-console-log-b94e4d248ed8)
## 如何正确的使用 console ？
1. 任何 console 代码都不能在线上存在或执行
    - eslint 检测 console ，在 husky 的 pre-commit 中强制跑 eslint，存在 console，阻止提交
    - terserPlugin
    - `console.log = function (){}`
2. 分环境和情况使用 console
  假如想要打印一些 warning、error 信息，或者想要在控制台打印招聘信息 ，此时不能一味的删除 `console`，可以合理的使用 `console` 中的 `log` 、`info`、`waring`、`error` 方法 ，此时可以分以下几种情况

    1. 测试环境下想看 而线上不想看 的调试信息用 log，
    2. 在测试环境和线上环境都想看到的一些信息用`info`、`waring`、`error`
    
    遵守上述规则后，用 babel 在 log 外部套一层 if 判断，方可达到想要的效果
     >  if (process.env.NODE_ENV !== "production") {
              console.log(456)
         }
         具体实现见：https://github.com/bowlingQ/babel-plugin-wrapper-dev