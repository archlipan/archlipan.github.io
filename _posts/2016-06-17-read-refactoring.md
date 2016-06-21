---
layout: post
title: 读重构
category: others
keywords: [book, software]
---

重构的思想，重构的手法，重构需要日常不断实践之。重构是什么？为什么要重构？如何判断哪些地方需要重构？


###  将重构当作一种日常

####  第一章节重构案例：重构是什么？

大部分情况下，编译器在不同代码结构下的运行效率并无太大差别，那么为什么我们要追求更加优雅的设计？因为代码不是一成不变的，需求总是在更新，代码在维护，如果每一次写代码都要推翻前人所有代码去更新，是不明智的，所以代码最终的面向对象依旧是人，靠人去维护更新，而人都是追求美的，优雅的结构，清晰的逻辑，艺术的代码；

重构的基础，第一步是构建测试环境，验证在重构过程中是否引入新的Bug；

重构函数，函数功能的细分，单一职责，越小的模块更加容易管理；

重构，逐步迭代处理，每一次更改的部分很小，故而容易校验错误的出现，进而避免了大规模的Debug，浪费过多时间；**重构的本质——以微小的步伐修改程序**

在逐步修改过程中，如果合适的变量名的修改是提升代码清晰的关键；

**优秀的程序员写出人容易理解的代码，而不是仅仅机器理解的代码**


根据函数功能的相关性，重构散乱无序的代码块；重构应该时刻进行，与功能的添加并行进行；

基于其他对象属性的Switch无法控制，不符合结构；如果Switch无可避免，理应针对本对象属性；

Case Type 模式转换为 State 模式构造，结合抽象与继承； ———— 这里看出**合理的重构还要对于设计模式有充分的理解**；

PS:这里有一个矛盾，重构对于前期的不合理点进行了修正，那么有人说我前期尽可能的考虑得更加完善，但是前期过度设计拖慢节奏并不是一个很好的点，初期快速的迭代加上及时的重构才是正途，进一步说明了重构的重要性，重构是一种以不变应万变的方案；



####  第二章节：重构原则

重构基础：不改变软件可观察性；

重构本质是对软件内部结构的调整，提高其可理解性；开发过程中的两个分支—— 重构与新功能实现，在不断切换中螺旋前进，但是在分支切换时，要清楚当前所处在的分支 —— 弄清当前的任务究竟是新增功能还是重构结构；

**重构的目的：**

开发过程中，软件的设计会逐渐腐朽变质，如果没有重构来整理代码，减少代码结构的流失；没有重构会恶性循环，越难看明白整体代码结构，程序腐败的速度越快，最终趋近不可控；

随着时间的流失，自己是很难记住自己写过的代码，合理的利用重构可以使代码维护时，迅速切换情景，找到当初的上下文环境，进行后续的更新维护；—— 重构可以擦掉程序中的污渍，让你在整体结构上看得更远；

不要特意抽出时间重构，重构应该无时无刻不伴随在软件开发过程中，添加功能时的重构，BugFix时的重构，以及Review代码时的重构；

**根本上要意识到有活力的软件是持续改变的，不要为了完成今天的功能而忘记明天还要维护更新迭代，以至于明天的工作无法开展**

重构过程中的中间层问题 —— 合理使用，防止中间层过度膨胀，及时清理，中间层的合理利用可以使结构更加具有弹性；




####  第三章节：针对哪些问题重构？




####  第四章节： 构建测试环境



###  核心章节：重构手法




### 总结

随时发现问题，随时重构，不断实践，适时停止，重构失控时回退再来；

牢记重构的目标： 使代码的功能不增不减；保持功能性；



---

待更新