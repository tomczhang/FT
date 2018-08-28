# 1. 生命周期
* 类调用
  * getDefaultProps
* 实例化
  * getInitState
  * componentWillMount
  * render
  * componentDidMount
* 变更
  * componentWillReceiveProps
  * shouldComponentUpdate
  * componentWillUpdate
  * render
  * componentDidUpdate
* 卸载
  * componentWillUnmount

# 2. React diff
传统Dom diff算法的复杂度是O(N^3)，React diff算法的复杂度为O(N)
* 原理：
tree diff比较：分层求异。只会比较树的同一层级，若同一层级出现不同，则会将这个节点及其子节点完全删掉，不会进一步比较，从而保证一遍循环就遍历完整颗树
component diff比较：相同类生成相同的树结构，不同类生成不同的树结构。如果是不同类型的组件，替换这个组件及其所有子节点，如果是相同类型的组件，继续按照diff算法比较，用户也可以使用shouldComponentUpdate()来判断组件是否需要进行diff。
element diff比较：设置唯一key。对于同一层级的节点，进行移动，插入和删除操作。用户可以对同一层级的节点添加唯一且稳定的key进行区分，从而提升效率。

# 3. React优化
* 避免DOM
  * shouldComponentUpdate
  * immutable.js