<template>
  <svg />
</template>

<script>
import * as d3 from "d3";
import { cars } from "../data/cars";

export default {
  name: "PieChart",
  mounted() {
    const color = [
      d3.rgb(214, 39, 40),
      d3.rgb(31, 119, 180),
      d3.rgb(174, 199, 232),
      d3.rgb(255, 127, 14),
      d3.rgb(255, 187, 120),
      d3.rgb(44, 160, 44),
      d3.rgb(152, 223, 138),
      d3.rgb(152, 50, 138)
    ];
    const sameMake = {};
    cars.forEach(e => {
      sameMake[e.make] = sameMake[e.make] ? sameMake[e.make] + 1 : 1;
    });

    const total = cars.length;

    const data = Object.keys(sameMake).map(e => ({
      make: e,
      percentage:
        sameMake[e] === 0 ? 0 : ((sameMake[e] / total) * 100).toFixed(1)
    }));

    const svgWidth = 400;
    const svgHeight = 400;
    const radius = Math.min(svgWidth, svgHeight) / 2;
    const svg = d3
      .select("svg")
      .attr("width", svgWidth)
      .attr("height", svgHeight);

    const g = svg
      .append("g")
      .attr("transform", "translate(" + radius + "," + radius + ")");

    const pie = d3.pie().value(d => d.percentage);

    const path = d3
      .arc()
      .outerRadius(radius)
      .innerRadius(0);

    const arc = g
      .selectAll("arc")
      .data(pie(data))
      .enter()
      .append("g");

    arc
      .append("path")
      .attr("d", path)
      .attr("fill", (d, i) => color[i]);

    const label = d3
      .arc()
      .outerRadius(radius)
      .innerRadius(0);

    arc
      .append("text")
      .attr("transform", d => `translate(${label.centroid(d)})`)
      .attr("text-anchor", "middle")
      .text(d => `${d.data.make}:${d.data.percentage}%`);
  }
};
</script>
