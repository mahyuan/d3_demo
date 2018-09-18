<style>
	button {
		width: auto;
		height: 26px;
		font-size: 18px;
	}
</style>

<template>
	<div class="d3">
		<button @click="blue">blue</button>
		<button @click="bck">background</button>
		<button @click="shados">shados</button>
		<button @click="arrdata">arrdata</button>
		<button @click="enter">enter</button>

		<p class="item item1">第1个项</p>
		<p class="item item2">第2个项</p>
		<p class="item item3">第3个项</p>
		<p class="item item4">第4个项</p>
		<p class="item item5">第5个项</p>
	</div>
</template>

<script>
	let d3 = require('d3');

	export default {
		name: 'dthree',
		data() {
			return {
				msg: 'unregistered',

			}
		},
		methods: {
			init() {
				var paragraphs = document.getElementsByTagName("p");
					for (var i = 0; i < paragraphs.length; i++) {
					  var paragraph = paragraphs.item(i);
					  paragraph.style.setProperty("color", "red", null);
					}

			},
			blue() {
				d3.selectAll('.item').style('color', 'blue');
				d3.select(".item2").style("background-color", "#eee");
			},
			bck() {
				d3.selectAll(".item").style("color", function() {
				  return "hsl(" + Math.random() * 360 + ",100%,50%)";
				});

			},
			shados () {
				d3.selectAll("p").style("color", function(d, i) { // ele, index
				  return i % 2 ? "#f0f" : "#ee0";
				});
				setTimeout( () => {
					var p = d3.select("body")
					  .selectAll("p")
					  .data([4, 8, 15, 16, 23, 42])
					    .text(function(d) { return d; });
		    }, 1000)

			},
			arrdata() {
				d3.selectAll("p")
				.data([4, 8, 15, 16, 23, 42])
				  .style("font-size", function(d) { return d + "px"; });
			},
			enter() {
				d3.select("body")
				  .selectAll("p")
				  .data([4, 8, 15, 16, 23, 42])
				  .enter().append("p")
				    .text(function(d) { return "I’m number " + d + "!"; });
		  },





		},
		mounted() {
			this.$nextTick(() => {
				this.init()
			})
		},
		computed: {

		}


	}
</script>