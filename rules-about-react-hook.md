# Why does the React require "Only Call Hooks at the Top Level"

#### &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;hello，hello，如果你使用react官方的hook：useState,useEffect，或者你尝试编写自己的hook，你也许会注意到这一条：

#### [在最顶层使用 Hook（Only Call Hooks at the Top Level）](https://reactjs.org/docs/hooks-rules.html) 为什么会有这样的要求呢？

#### &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;首先Hook用于function组件中, 能够保持function组件的状态。在react@16.8以后,

#### 官方开始推荐使用Hook语法, 常用的 api 有useState,useEffect,useCallback等, 官方一共定义了 ["14 种Hook类型"](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiberHooks.old.js#L111-L125). 这些 api 背后都会创建一个

#### Hook对象, 一个hook[对象会包括：](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiberHooks.old.js#L134-L140)

####

    export type Hook = {|
    memoizedState: any,
    baseState: any,
    baseQueue: Update<any, any> | null,
    queue: UpdateQueue<any, any> | null,
    next: Hook | null,
    |};

    type Update<S, A> = {|
    lane: Lane,
    action: A,
    eagerReducer: ((S, A) => S) | null,
    eagerState: S | null,
    next: Update<S, A>,
    priority?: ReactPriorityLevel,
    |};

    type UpdateQueue<S, A> = {|
    pending: Update<S, A> | null,
    dispatch: (A => mixed) | null,
    lastRenderedReducer: ((S, A) => S) | null,
    lastRenderedState: S | null,
    |};

属性解释:

Hook memoizedState: 内存状态, 用于输出成最终的fiber树 (fiber是一个在reconciler里的对象)。  
baseState: 基础状态, 当Hook.queue更新过后, baseState也会更新。   
baseQueue:基础状态队列, 在reconciler阶段会辅助状态合并。  
queue: 指向一个Update队列。

### next: 指向该function组件的下一个Hook对象, 使得多个Hook之间也构成了一个链表。

### 所以，当我们有多个hook，function 在多次执行的时候，hook的指向发生变化，就不能正确的执行啦！

Hook.queue和 Hook.baseQueue(即UpdateQueue和Update）是为了保证Hook对象能够顺利更新,fiber.updateQueue中的UpdateQueue和  
Update是不一样的(且它们在不同的文件)。  
(Hook与fiber的关系:在fiber对象中有一个属性fiber.memoizedState指向fiber节点的内存状态. 在function类型的组件中,  
fiber.memoizedState就指向Hook队列(Hook队列保存了function类型的组件状态))。
