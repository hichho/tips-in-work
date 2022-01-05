# React的渲染次数

&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;在很多时候，我们希望去获取React的render次数，查阅了React官方文档，好像没有给出直接  
的api。

#### 我想了一个方案：['由于useRef在组件的整个生命周期内保持不变'](https://react.docschina.org/docs/hooks-reference.html#useref) 所以我使用了ref.current来保存该函数组件执行的次数。

但是在使用调试过程中发现，除开第一次，每更新一下dom，ref 的值变动两次（如果+1,执行两次会+2)。 我的猜测是：react包  
在调用react-reconciler时，react-reconciler执行了一次，而后react-reconciler在scheduler中注册了调度任务， 接着调度任务  
执行回调函数时，又执行了一次。  
(todo)
