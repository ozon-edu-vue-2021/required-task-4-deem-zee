<template>
    <div id="app">	
  <h1>Custom Select Dropdown</h1>

  <div class="aselect" :data-value="value" :data-list="list">
	    <div class="selector" @click="toggle()">
	        <div class="label">
				    <span>{{ value }}</span>
	        </div>
			<div class="arrow" :class="{ expanded : visible }"></div>
	        <div :class="{ hidden : !visible, visible }">
	            <ul>
	                <li :class="{ current : item === value }" v-for="item in list" @click="select(item)" :key="item">{{ item }}</li>
	            </ul>
	        </div>
	    </div>
	</div>
</div>
</template>

<script>
export default {
    el: "#app",
		name: 'aselect',
		data: {
			value: 'Select a Fruit',
			list: ["Orange","Apple","Kiwi", "Lemon", "Pineapple"],
      visible: false
		},
		methods: {
			toggle() {
				this.visible = !this.visible;
			},
			select(option) {
			    this.value = option;
			}
		}
}
</script>

<style>

</style>