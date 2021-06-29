---
title: 学习toolkit的基本用法
---

# 目的
**Redux工具包** 致力于成为编写 Redux 逻辑的标准方式。它最初是为了帮助解决有关 Redux 的三个常见问题而创建的：
+ "配置一个 Redux store 过于复杂"
+ "做任何 Redux 的事情我都需要添加很多包"
+ "Redux 需要太多的样板代码"

# 包含内容
Redux工具包 包含了如下API:

+ <font color="#764abc"> configureStore() </font>: 包装 createStore 以提供简化的配置选项和良好的默认预设。它可以自动组合你的切片 reducers，添加您提供的任何 Redux 中间件，默认情况下包含 redux-thunk ，并允许使用 Redux DevTools 扩展。
+ <font color="#764abc">createReducer()</font>: 为 case reducer 函数提供 action 类型的查找表，而不是编写switch语句。此外，它会自动使用immer 库来让您使用普通的可变代码编写更简单的 immutable 更新，例如 state.todos [3] .completed = true 。
+ <font color="#764abc">createAction()</font>: 为给定的 action type string 生成一个 action creator 函数。函数本身定义了 toString()，因此它可以用来代替 type 常量。
+ <font color="#764abc">createSlice()</font>: 接受一个 reducer 函数的对象、分片名称和初始状态值，并且自动生成具有相应 action creators 和 action 类型的分片reducer。
+ <font color="#764abc">createAsyncThunk </font>: 接受一个 action type string 和一个返回 promise 的函数，并生成一个发起基于该 promise 的pending/fulfilled/rejected action 类型的 thunk。
+ <font color="#764abc">createEntityAdapter </font>: 生成一组可重用的 reducers 和 selectors，以管理存储中的规范化数据
+ <font color="#764abc">createSelector 组件 </font> 来自 Reselect 库，为了易用再导出。

# 使用方法:

### 同步redux
> 使用切片,不然写在一起太多

```js
export const counterSlice = createSlice({
    name: 'counter', // 每个部分的名称
    initialState, // 初始化值
    reducers: {
    // reduces 因为state是响应式的(使用proxy API), 所以没有返回值(void)!!!!
        increment: (state) => { state.value ++ } ,
        decrement: state => {
            if (state.value === 0) {
                state.value = 0
            }else{
                state.value --
            }
        },
        incrementByAmount: (state, action) => {
            state.value += action.payload
        }
    },
    extraReducers: (builder) => {
        builder.addCase(incrementAsync.fulfilled, (state, action) => {
            state.value += action.payload
        } )
    }
})
```

#### 异步redux
> 使用createAsyncThunk, 第一个参数是name, 第二个是callback 

- callback的返回值是成为“已完成”操作负载
- 在 createSlice当中手动extraReducers异步任务, (也可以自动添加任务)
```js
extraReducers: (builder) => {
// 使用addCase添加异步任务,因为是个promise,所以有pending,fulfilled, reject三个阶段, addCase第一个参数是异步任务的状态, 第二个是reducer
    extraReducers: (builder) => {
        builder.addCase(incrementAsync.pending, (state) => {
            state.status = '等待中'
        })
```

```js

        .addCase(incrementAsync.fulfilled, (state, action) => {
            state.status = '完成✅'
            state.value += action.payload
        } )
    },
```
```js
export const incrementAsync = createAsyncThunk('counter/incrementAsync',
  async (amount) => {
      const response = await fetchCount(amount)
      return response.data
  }
)

```