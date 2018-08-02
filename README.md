# vue-checkbox
# 基于vue的checkbox组件，不依赖其他第三方库
# 属性          类型          描述
# options       array        必填
# label         string       默认为label
# selected      array        默认所选中项（选中项对应的索引数组），必填
# direction     string       默认horizontal（水平方向），可选值vertical（垂直方向）
# relkey        string       索引值，默认为id，若索引不为id，则必填
# 事件       参数
# change     val，为所选项索引数组

# 后续优化增加最大可选数量，勾号颜色，以及背景色

# 有需要其他需求的 加我微信（qq）591339209说明

# 用法
# <check-list :options="printer.operate" :selected="printer.selected" @change="select" style="margin-top:-10px;"></check-list>
