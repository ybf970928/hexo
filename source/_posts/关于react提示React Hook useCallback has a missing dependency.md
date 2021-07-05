---
title: 在useEffect的回调函数中执行某个function时会提示 React Hook useCallback has a missing dependency 
---

#### 原因: 
首先，不需要返回在useEffect钩子中调用fetchData()函数的结果。

现在谈到您的问题，您得到警告的原因是因为缺少useEffect的依赖项可能会导致闭包导致错误。React建议不要忽略useEffect钩子、useCallback钩子等的任何依赖项。

这有时会导致状态更新和re-render的无限循环，但这可以通过使用useCallback钩子或其他方法来防止useEffect钩子在组件的每个re-render之后执行。

可以通过以下方式解决问题：
<!--more-->
将fetchData函数包装到useCallback钩子const fetchData = useCallback(async (params) => { const res = await axios.get('/url', { params }); appDispatch({ type: "LOAD_ELEMENTS", elements: res.data }); }, []);
在useEffect钩子useEffect(() => { fetchData(); }, [fetchData]); 的依赖数组中添加fetchData

> 参考链接: https://www.5axxw.com/questions/content/sgwya7