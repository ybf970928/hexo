---
title: 商品sku 的全排列算法
---
##### 直接贴代码: 

``` js
const data = [
        ['大陆版', '美版', '港版'],
        ['红色', '黑色', '白色'],
        ['32g', '64g', '128g', '256g']
      ]
      const temp = data.reduce((total, current) => {
        return (
          total.map(total => {
            return (current.map(cur => {
              return cur + total
            }))
            // 返回是的data.length长度的数组
          }).reduce((total, curr) =>
            // 把每一项item合并成一个数组
          [...total, ...curr])
        )
      })
```