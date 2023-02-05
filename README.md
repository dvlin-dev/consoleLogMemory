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

