### 更新
#### 新增是否开启全选功能，属性为encheckall，值为true时开启，非必填，默认为false
#### 新增最大可选项数控制，属性为max值为小于等于options数组长度的的数值，非必填，默认为options数组长度
#### 废弃selected属性，采用v-model实现数据双向绑定

### 注意事项
#### 1.全选功能不能与最大可选项数控制功能同时开启
#### 2.开启全选功能时，点击全选那一项时，父组件@change事件回调参数将不包括data.current一项，需进行特殊处理以防止页面报错
#### 3.开启最大可选项数控制时，达到最大可选数量时，点击其他未选项将不再触发父组件@change事件，如需触发请自行优化

### vue-checkbox
#### 基于vue的checkbox组件，不依赖其他第三方库


### 属性          类型          描述
##### options       array        必填
##### label         string       默认为label
##### v-moel      array          默认所选中项（选中项对应的索引数组），必填
##### direction     string       默认horizontal（水平方向），可选值vertical（垂直方向）
##### relkey        string       索引值，默认为id，若索引不为id，则必填
##### encheckall    boolean      非必填，默认为false，值为true时开启
##### max           number       最大可选项数，非必填，默认为options数组长度，值为小于等于options数组长度的的数值


### 事件       参数
#### change     data， data.val为所选项索引数组,data.current为当前所点击项的数据


#### 后续优化增加最大可选数量，勾号颜色，以及背景色

#### 有需要其他需求的 加我微信（qq）591339209说明

### 用法
### <check-list :options="printer.operate" :selected="printer.selected" @change="select" style="margin-top:-10px;"></check-list>
