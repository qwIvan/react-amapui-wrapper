# react-amapui-wrapper

> 一个令人兴奋的消息，react-amap 可以在内部load amapui 了，这样就可以不用我自己写的apiloader了.

---
## 简介
> react-amapui-wrapper 是一个基于 react-amap 封装的高德地图组件；主要是用于处理 react-amap 的 AMapUI 的部分。

---
## 如何使用
```JavaScript
import React,{ Component } from 'react'
import ReactDOM from 'react-dom'
import { Map } from 'react-amap'
import { Wrapper, DistrictExplorer } from 'react-amapui-wrapper'

class MapUIComponent extends Component {
  render() {
    return <Map>
      <Wrapper>
        <DistrictExplorer> 
          events = {} 
          eventSupport = {} 
          instanceName = {'DistrictExplorer'}
        /> 
      /> 
    />
  }
}

ReactDOM.render(
  <MapUIComponent/>,
  document.querySelector('#app')
)
```
---
## 相关配置项

`events`为事件配置项，事件绑定方式与[react-amap中绑定方式](https://github.com/ElemeFE/react-amap/blob/master/components/about.md)相同
`instanceName`为从高德地图中load的地图元素，具体填入内容参照高德地图AMapUI。

---

## 如何自定义AMapUI组件
```JavaScript
import React,{ Component } from 'react'
import { Base } from 'react-amapui-wrapper'

// 自定义组件
class newCp extends Base {
// updateProps 用于更新 props, 可自定义props改变
  updateProps(nextProps,thisProps) {
    
  }

// initialInstance 用于初始化组件, 可自定义如何初始化组件
  initialInstance() {
    const { eventSupport=false } = this.props
    if (this[this.instanceName]) {
      return new Promise((resolve) => {
        resolve(this[this.instanceName])
      })
    } else {
      return new Promise((resolve) => {
        this.amapui.load(['ui/geo/DistrictExplorer'], (DistrictExplorer) => {

          this[this.instanceName] = new DistrictExplorer({
            eventSupport: eventSupport,
            map: this.map //关联的地图实例
          })
          const events = this.exposeInstance(this.props)
          events && this.bindEvents(events)
          this.initPage()
          resolve(this[this.instanceName])
        })
      })
    }
  }
export default newCp
```

---
## todo

* [ ] 1. 将组件的继承方式改成通过hoc继承
* [ ] 2. 为各个组件添加static props和dynamic props