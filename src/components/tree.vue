<style>
  #tree_contaienr {
    display: block;
  }

  svg {
    background-color: #f5f5f5;
  }

  .node circle {
    fill: #999;
  }

  .node text {
    font: 10px sans-serif;
  }

  .node--internal circle {
    fill: #555;
  }

  .node--internal text {
    text-shadow: 0 1px 0 #fff, 0 -1px 0 #fff, 1px 0 0 #fff, -1px 0 0 #fff;
  }

  .link {
    fill: none;
    stroke: #555;
    stroke-opacity: 0.4;
    stroke-width: 1.5px;
  }
</style>

<template>
  <svg width='960' height='2000'></svg>
  <!-- <div id='tree_contaienr'>
  	</div> -->
</template>

<script>
  let d3 = require('d3');
  let info = require('../assets/flare.csv');
  // let info = require('../assets/data.js');

  export default {
    name: 'tree',
    data() {
      return {
        info,
        msg: 'tree svg',
      }
    },
    methods: {
      csvJSON(csv) {
        var lines=csv.split("\n");

        var result = [];

        var headers=lines[0].split(",");

        for(var i=1;i<lines.length;i++){

          var obj = {};
          var currentline=lines[i].split(",");

          for(var j=0;j<headers.length;j++){
            obj[headers[j]] = currentline[j];
          }

          result.push(obj);

        }

        //return result; //JavaScript object
        return JSON.stringify(result); //JSON
      },
      async init() {
        let _data = this.csvJSON(this.info);

        console.log('init start');
        console.log('_data', _data);

        let svg = d3.select('svg');
        let width = +svg.attr('width');
        let height = +svg.attr('height');
        let g = svg.append('g').attr('transform', 'translate(40, 0)');
        let tree = d3.cluster().size([height, width - 160]);

        console.log('1');
        let stratify = d3.stratify().parentId(d => d.id.substring(0, d.id.lastIndexOf('.')))

        console.log('info', this.info);


        await d3.csv( '../assets/flare.csv',  (error, data) => {
          console.log('2 error', error);
          console.log('data', data);

          if (error) throw error;

          let root = stratify(data).sort((a, b) => (a.height - b.height) || a.id.localeCompare(b.id))

          console.log('3');
          tree(root);
          console.log('4');

          let link = g.selectAll('.link')
            .data(root.descindants()
              .slice(1))
            .enter()
            .append('path')
            .attr('class', 'link')
            .attr('d', function(d) {
              return "M" + d.y + "," + d.x +
                "C" + (d.parent.y + 100) + "," + d.x +
                " " + (d.parent.y + 100) + "," + d.parent.x +
                " " + d.parent.y + "," + d.parent.x;
            })

          var node = g.selectAll(".node")
            .data(root.descendants())
            .enter().append("g")
            .attr("class", function(d) {
              return "node" + (d.children ? " node--internal" : " node--leaf");
            })
            .attr("transform", function(d) {
              return "translate(" + d.y + "," + d.x + ")";
            })

          node.append("circle")
            .attr("r", 2.5);

          node.append("text")
            .attr("dy", 3)
            .attr("x", function(d) {
              return d.children ? -8 : 8;
            })
            .style("text-anchor", function(d) {
              return d.children ? "end" : "start";
            })
            .text(function(d) {
              return d.id.substring(d.id.lastIndexOf(".") + 1);
            });

        })

      }
    },
    mounted() {
      console.log('mounted start');
      this.init();
    }
  }
</script>
