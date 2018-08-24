<template>
	<ul class="checkbox">
		<li :class="[initDirection == 'horizontal' ? 'checkbox-item-horizontal' : 'checkbox-item-vertical']" v-if="initEnCheckAll">
			<span @click="selectAll()" :class="['checked', initSelectedAll ? 'checked-active' : '']"></span>
			<div @click="selectAll()" class="check-label">
				全选
			</div>
		</li>
		<li :class="[initDirection == 'horizontal' ? 'checkbox-item-horizontal' : 'checkbox-item-vertical']" v-for="(i, index) in initOptions">
			<span @click="select(index)" :class="['checked', i.checked ? 'checked-active' : '']"></span>
			<div @click="select(index)" class="check-label">
				{{initLabel ? i[initLabel] : i}}
			</div>
		</li>
	</ul>
</template>
<script>
	export default {
		props: ['options','selected','bgc','direction','max','label','checked','relkey','encheckall'],
		data () {
			return {
				isMax:false,
				initSelectedAll: false,
				// initOptions: this.options ? this.options : [],
				initSelected: this.selected ? this.selected : [],
				initBgc: this.bgc ? this.bgc : '#FFBF2F',
				initDirection: this.direction ? this.direction : 'horizontal',
				initMax: this.max ? Number(this.max) : this.options.length,
				initLabel: this.label ? this.label : 'label',
				initChecked: this.checked ? this.checked : 'checked',
				initRelKey: this.relkey ? this.relkey : 'id',
				initEnCheckAll: this.encheckall ? this.encheckall : false
			}
		},
		methods: {
			selectAll () {
				this.initSelectedAll = !this.initSelectedAll
				if (this.initSelectedAll) {
					this.initSelected = []
					this.initOptions.forEach(e => {
						this.initSelected.push(e[this.initRelKey])
					})
				} else {
					this.initSelected = []
				}
				this.init()
				this.$emit('change', {val:this.initSelected})
			},
			select (index) {
				console.log(index, 'index')
				if (!this.initOptions[index].checked && this.isMax) return
				this.initOptions[index].checked = !this.initOptions[index].checked
				let a = this.initOptions[index]
				this.initOptions.splice(index, 1, a)
				if (!this.initOptions[index].checked) {
					this.initSelected.forEach((e, index) => {
						if (e == a[this.initRelKey]) {
							this.initSelected.splice(index,1)
						}
					})
				} else {
					this.initSelected.push(a[this.initRelKey])
				}
				this.$emit('change', {val:this.initSelected, current: this.initOptions[index]})
			},
			init () {
				let v = this
				if (v.initOptions.length > 0) {
					v.initOptions.forEach((eg, index) => {
						if (v.initSelected.length > 0) {
							eg.checked = false
							v.initSelected.forEach(item => {
								// console.log(item, 'item')
								// console.log(v.initRelKey, 'eg[v.initRelKey]')
								if (item == eg[v.initRelKey]) {
									eg.checked = true
									let a = eg
									v.initOptions.splice(index, 1, a)
								}
							})
						} else {
							if (eg.checked) {
								let a = eg
								a.checked = false
								v.initOptions.splice(index, 1, a)
							}
							eg.checked = false
						}
					})
				}
				console.log(v.initOptions, 'v.initOptions')
			}
		},
		computed: {
			initOptions () {
				let initOptions = this.options ? this.options : []
				return initOptions
			}
		},
		watch: {
			selected: function (val ,oldVal) {
				let v = this
				console.log(val, 'val')
				v.initSelected = val
				v.init()
			},
			initSelected: function (val, oldVal) {
				if (val.length != this.initOptions.length) {
					this.initSelectedAll = false
				} else {
					this.initSelectedAll = true
				}
				if (val.length >= this.initMax) {
					this.isMax = true
				} else {
					this.isMax = false
				}
			}
		},
		mounted () {
			console.log(111111)
			let v = this
			v.initSelected = this.selected
			// console.log(v.initSelected, 'v.initSelected')
			v.init()
			if (this.initSelected.length != this.initOptions.length) {
				this.initSelectedAll = false
			} else {
				this.initSelectedAll = true
			}
			if (this.initSelected.length >= this.initMax) {
				this.isMax = true
			} else {
				this.isMax = false
			}
		}
	}
</script>
<style scoped>
	.checkbox {
		margin-bottom: 0;
	}
	.checkbox-item-horizontal {
		font-size: 0;
		display: inline-block;
		line-height: 35px;
	}
	.checkbox-item-vertical {
		font-size: 0;
		line-height: 35px;
	}
	.checked {
		display: inline-block;
		width: 15px;
		height: 15px;
		box-sizing: border-box;
		vertical-align: middle;
		border: 1px solid #6F6F6F;
		border-radius: 50%;
		cursor: pointer;
	}
	span.checked-active {
		background-color: #FFBF2F;
		border:none !important;
	}
	.checked-active:after {
		font-size:14px;
		content:"\2714";
		color: #FFF;
		position: relative;
		top: -10px;
		left: 2px;
	}
	.checkbox-item-horizontal .checked-active:after {
		top: -10px;
	}
	.check-label {
		display: inline-block;
		font-size: 14px;
		vertical-align: middle;
		color: #292929;
		margin: 0 5px;
		margin-right: 20px;
		cursor: pointer;
		-webkit-user-select:none;
     	-moz-user-select:none;
     	-ms-user-select:none;
     	user-select:none;
	}
</style>