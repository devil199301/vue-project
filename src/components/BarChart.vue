<template>
  <svg />
</template>

<script>
import * as d3 from "d3";
import { cars } from "../data/cars";

export default {
  name: "BarChart",
  mounted() {
    const sameSize = {};
    cars.forEach(e => {
      sameSize[e.size] = sameSize[e.size] ? [...sameSize[e.size], e] : [e];
    });
    const data = Object.keys(sameSize)
      .map(e => ({
        size: e,
        average:
          Math.round(
            (sameSize[e].map(e => e.kw).reduce((a, b) => a + b) /
              sameSize[e].length) *
              100
          ) / 100
      }))
      .sort((a, b) => a.average - b.average);

    // 設定 svg 的寬高
    const chart = d3
      .select("svg")
      .attr("width", 750)
      .attr("height", 250);

    // 選取所有的 g 群組
    const group = chart
      .selectAll("g")
      .data(data)
      .enter()
      .append("g");

    // 取比例尺
    const scale = d3
      .scaleLinear()
      .domain([0, d3.max(data.map(item => item.average))])
      .range([0, 500]);

    // 繪製長條圖
    group
      .append("rect")
      .attr("x", 200)
      .attr("y", (item, i) => i * 25)
      .attr("width", item => scale(item.average))
      .attr("height", 25 - 2)
      .attr("fill", "steelblue");

    // 左邊標題
    group
      .append("text")
      .attr("x", 0)
      .attr("y", (item, i) => i * 25 + 18)
      .style("fill", "black")
      .style("font-size", "15px")
      .style("font-weight", "bold")
      .text(item => item.size);

    // 平均值
    group
      .append("text")
      .attr("x", item => 200 + scale(item.average))
      .attr("y", (item, i) => i * 25 + 18)
      .attr("text-anchor", "end")
      .style("fill", "black")
      .style("font-size", "18px")
      .style("font-weight", "bold")
      .text(item => item.average);

    // 顯示坐標軸
    const axis = d3
      .axisBottom(scale)
      .ticks(5)
      .tickFormat(item => item);

    chart
      .append("g")
      .call(axis)
      .attr("transform", "translate(200,180)");
  }
};
</script>
