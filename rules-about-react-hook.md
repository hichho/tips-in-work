# Why does the React require "Only Call Hooks at the Top Level"

#### &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;hello，hello，如果你使用react官方的hook：useState,useEffect，或者你尝试编写自己的hook，你也许会注意到这一条：

#### [在最顶层使用 Hook（Only Call Hooks at the Top Level）](https://reactjs.org/docs/hooks-rules.html) 为什么会有这样的要求呢？

#### &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;首先Hook用于function组件中, 能够保持function组件的状态。在react@16.8以后, 官方开始推荐使用Hook语法, 常用的 api 有useState,

useEffect,useCallback等,
官方一共定义了 ["14 种Hook类型"](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiberHooks.old.js#L111-L125)
. 这些 api 背后都会创建一个 Hook对象,
一个hook[对象会包括：](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiberHooks.old.js#L134-L140)

    export type Hook = {|
    memoizedState: any,
    baseState: any,
    baseQueue: Update<any, any> | null,
    queue: UpdateQueue<any, any> | null,
    next: Hook | null,
    |};

其中:

Hook memoizedState: 内存状态, 用于输出成最终的fiber树 (fiber是一个在reconciler里的对象)。  
baseState: 基础状态, 当Hook.queue更新过后, baseState也会更新。   
baseQueue:基础状态队列, 在reconciler阶段会辅助状态合并。  
queue: 指向一个Update队列。

### next: next的类型可以是null或者Hook类型，其实它指向该function组件的下一个Hook对象, 使得多个Hook之间也构成了一个链表。

### 所以，当我们有多个hook，function 在多次执行的时候，hook的指向发生变化，就不能正确的执行啦！

#### ps:😂hook 为什么叫hook：其实hook 的概念应用很广泛（好像c#，安卓，操作系统中都有），它其实就是"封装了一层逻辑，并钩入当前函数！"
