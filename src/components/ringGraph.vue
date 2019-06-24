<template>
  <div id="graphWrapper">
    {{types}}
  </div>
</template>

<script>
import * as d3 from "d3";
export default {
  data: function() {
    return {
      width: 960,
      height: 450,
      radius: 225,
      pie: d3.layout
        .pie()
        .sort(null)
        .value(function(d) {
          return d.value;
        }),
      arc: d3.svg
        .arc()
        .outerRadius(this.radius * 0.8)
        .innerRadius(this.radius * 0.4),
      outerArc: d3.svg
        .arc()
        .innerRadius(this.radius * 0.9)
        .outerRadius(this.radius * 0.9),
      color: d3.scale
        .ordinal()
        .domain([
          "label1",
          "label2",
          "label3",
          "label4",
          "label5",
          "label6",
          "label7",
          "label8",
          "label9",
          "label10"
        ])
        .range(["#98abc5", "#8a89a6", "#7b6888"])
    };
  },
  computed: {
    svg: function() {
      var svg = d3
        .select("#graphWrapper")
        .append("svg")
        .append("g")
        .attr(
          "transform",
          "translate(" + this.width / 2 + "," + this.height / 2 + ")"
        );

      svg.append("g").attr("class", "slices");
      svg.append("g").attr("class", "labels");
      svg.append("g").attr("class", "lines");
      return svg;
    }
  },
  methods: {
    key: function(d) {
      return d.data.label;
    },
    change: function(data) {
      /* ------- PIE SLICES -------*/
      var slice = this.svg
        .select(".slices")
        .selectAll("path.slice")
        .data(pie(data), key);

      slice
        .enter()
        .insert("path")
        .style("fill", function(d) {
          return color(d.data.label);
        })
        .attr("class", "slice");

      slice
        .transition()
        .duration(1000)
        .attrTween("d", function(d) {
          this._current = this._current || d;
          var interpolate = d3.interpolate(this._current, d);
          this._current = interpolate(0);
          return function(t) {
            return this.arc(interpolate(t));
          };
        });

      slice.exit().remove();

      /* ------- TEXT LABELS -------*/

      var text = svg
        .select(".labels")
        .selectAll("text")
        .data(pie(data), this.key);

      text
        .enter()
        .append("text")
        .attr("dy", ".35em")
        .text(function(d) {
          return d.data.label;
        });

      function midAngle(d) {
        return d.startAngle + (d.endAngle - d.startAngle) / 2;
      }

      text
        .transition()
        .duration(1000)
        .attrTween("transform", function(d) {
          this._current = this._current || d;
          var interpolate = d3.interpolate(this._current, d);
          this._current = interpolate(0);
          return function(t) {
            var d2 = interpolate(t);
            var pos = this.outerArc.centroid(d2);
            pos[0] = this.radius * (midAngle(d2) < Math.PI ? 1 : -1);
            return "translate(" + pos + ")";
          };
        })
        .styleTween("text-anchor", function(d) {
          this._current = this._current || d;
          var interpolate = d3.interpolate(this._current, d);
          this._current = interpolate(0);
          return function(t) {
            var d2 = interpolate(t);
            return midAngle(d2) < Math.PI ? "start" : "end";
          };
        });

      text.exit().remove();

      /* ------- SLICE TO TEXT POLYLINES -------*/

      var polyline = svg
        .select(".lines")
        .selectAll("polyline")
        .data(pie(data), key);

      polyline.enter().append("polyline");

      polyline
        .transition()
        .duration(1000)
        .attrTween("points", function(d) {
          this._current = this._current || d;
          var interpolate = d3.interpolate(this._current, d);
          this._current = interpolate(0);
          return function(t) {
            var d2 = interpolate(t);
            var pos = this.outerArc.centroid(d2);
            pos[0] = this.radius * 0.95 * (midAngle(d2) < Math.PI ? 1 : -1);
            return [arc.centroid(d2), this.outerArc.centroid(d2), pos];
          };
        });

      polyline.exit().remove();
    }
  },
  props: ["types"],
  watch: {
    types: function() {
      change(this.types);
    }
  }
};
</script>

<style scoped>
#graphWrapper {
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  width: 960px;
  height: 500px;
  position: relative;
}

svg {
  width: 100%;
  height: 100%;
}

path.slice {
  stroke-width: 2px;
}

polyline {
  opacity: 0.3;
  stroke: black;
  stroke-width: 2px;
  fill: none;
}
</style>
