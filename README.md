## 拆分结构和样式

1. 输入框为Header
2. 显示内容的为List，并把里面的每一项拆分为Item
3. 底部拆分为Footer

> 注意点：在拆分样式的时候注意把class和style更改

> 动态初始化列表，如何确定将数据放在哪个组件身上？

如果是自己使用的则放在自身的state身上

如果是一些组件都需要使用，则放在他们共同的父组件身上(官方称此操作为：状态提升)

> 关于父子之间的通信

- 父组件给子组件传递数据：通过props传递
- 子组件给父组件传递数据：通过props传递，要求父组件提前给子组件传递一个函数

> 注意点：defaultChecked和checked的区别：前者的值只在第一次时生效，而后者的则在表单改变时就会改变。在React中使用后者会报错，提示我们需要使用onChange事件把属性不写死

### Header组件

> 实现功能：输入的内容不为空，按下回车读取value，并渲染在List组件上，输入完成清空value

> 按下回车读取value并渲染到List组件

- 给表单注册一个键盘弹起事件(onKeyUp)，传入事件对象，判断键盘ASCII码值为13(回车)，调用父组件使用props传过来的函数，设置参数为对象类型，id使用nanoid库创建，name为value，done为false

> 输入的内容不能为空

if判断使用字符串的trim方法判断字符串是否为空

> 清空value

target.value = ''

> 父组件处理程序

接收传过来的参数，并获取自身的状态，然后创建一个新的数据，把接收过来的数据放在前面，原数据使用扩展运算符在后面展开，最后改变状态，把状态通过props传给List

### List组件

把接收过来的props传给Item组件

#### Item 组件

接收父组件传过来的props，并把他们渲染到页面中

> 实现功能：鼠标移出，当前项高亮并显示删除按钮，勾选或取消一个todo，删除一个todo



> 鼠标移入

给每个li都绑定移出移入事件，设置一个回调，且传入参数，移入为true，反之false。设置状态，初始值为false。函数内判断这个参数的值并设置给状态。

再给每一个li设置背景，判断状态，然后设置相应的背景色，显示删除按钮也是如此

> 勾选或取消一个todo

使用onChange事件，复选框发生改变时触发一个回调，并把这个回调的id传出。函数内调用父组件的一个函数，并把当前li的id和checked值传给父组件，然后父组件在进行修改他的状态

> 删除一个todo

给删除按钮注册一个点击事件，触发一个回调，并把当前项的id传出，回调内使用window.confirm判断是否删除，确定则调用父组件的函数并把这个id传给父组件，父组件在删除对应id的数据



### Footer组件

> 实现功能：全选或全不选，清除已完成的todo



> 全选或全不选

接收父组件传过来的props，并使用reduce方法计数。

第一个参数为上一次计算的结果，第二个参数为当前需要计算的元素，设置初始值为0 函数内判断数据的done属性是否为true，为true+1反之不加。给li设置checked属性，判断条件为完成的个数 == 数组的长度 && 数组的长度不为零

给表单组件onChange事件，回调内调用父组件传过来的函数，并把当前项的checked值传给父组件。父组件在更改数据的done值

> 清楚已完成的todo

给清除按钮注册一个点击事件，设置一个回调，回调内调用父组件的函数。父组件判断数据中done为false的，并更改状态



> ==此案例中，要显示的数据都放在了父组件state中，而每个组件自己需要设置的则在自身上设置state==

