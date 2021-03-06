# vue-virtual-collection源码分析

/*
  0 1 2 3 4 5     
 ┏━━━┯━━━┯━━━┓     
0┃0 0┊1 3┊6 6┃      
1┃0 0┊2 3┊6 6┃   
 ┠┈┈┈┼┈┈┈┼┈┈┈┨   
2┃4 4┊4 3┊7 8┃    
3┃4 4┊4 5┊9 9┃    
 ┗━━━┷━━━┷━━━┛     
 
Sections to Cells map:    
 0.0 [0]   
 1.0 [1, 2, 3]   
 2.0 [6]   
 0.1 [4]   
 1.1 [3, 4, 5]   
 2.1 [7, 8, 9]   
*/   
 

**该组件的思想:**  

为了高效率地计算 viewport 中有哪些 Cell 需要渲染，改用了“块渲染的思想”。我们可以定义一个“块”为 200 * 200 的正方形，所有与这个块有重叠的 Cell 都会在这个块中记录下来。 

这些“块”被保存在一个 Map 中，当滚动发生时，我们只需要计算当前该展示哪些块的数据，然后去这些块中找到对应的 Cell 就可以了，而不需要去遍历所有的 Cell。

**该组件功能:**   

现在自测大概可以无卡顿支持 100W+ 的数据渲染，大部分场景下肯定是够用了。原理上就是局部渲染和DOM 回收，不会渲染全部数据，而是把当前 viewport 中展示的 Cell 渲染出来，所以性能上比渲染全量数据要快太多了。

感谢[@starkwang](https://github.com/starkwang/vue-virtual-collection)提供这么好的一个插件
[本文参考](https://zhuanlan.zhihu.com/p/34380557)
