<template>
	<ul class="checkbox">
		<li :class="[initDirection == 'horizontal' ? 'checkbox-item-horizontal' : 'checkbox-item-vertical']" v-for="(i, index) in initOptions">
			<span @click="select(index)" :class="['checked', i.checked ? 'checked-active' : '']">
				
			</span>
			<div @click="select(index)" class="check-label">
				{{initLabel ? i[initLabel] : i}}
			</div>
		</li>
	</ul>
</template>
<script>
	export default {
		props: ['options','selected','bgc','direction','max','label','checked','relkey'],
		data () {
			return {
				// initOptions: this.options ? this.options : [],
				initSelected: this.selected ? this.selected : [],
				initBgc: this.bgc ? this.bgc : '#FFBF2F',
				initDirection: this.direction ? this.direction : 'horizontal',
				initMax: this.max ? this.max : this.options.length,
				initLabel: this.label ? this.label : 'label',
				initChecked: this.checked ? this.checked : 'checked',
				initRelKey: this.relkey ? this.relkey : 'id',
			}
		},
		methods: {
			select (index) {
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
				v.initSelected = val
				v.init()
			}
		},
		mounted () {
			let v = this
			v.initSelected = this.selected
			v.init()
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
		vertical-align: middle;
		border: 1px solid #6F6F6F;
		border-radius: 50%;
		box-sizing: border-box;
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