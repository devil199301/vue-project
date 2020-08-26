<template>
  <svg />
</template>

<script>
import * as d3 from "d3";
import { cars } from "../data/cars";

export default {
  name: "LineChart",
  mounted() {
    const sameYear = {};

    cars.forEach(e => {
      sameYear[e.year] = sameYear[e.year] ? sameYear[e.year] + 1 : 1;
    });

    const data = Object.keys(sameYear).map(e => ({
      year: e,
      count: sameYear[e]
    }));

    const svg = d3
      .select("svg")
      .attr("height", 300)
      .attr("width", 600);

    const margin = { top: 0, bottom: 20, left: 30, right: 20 };
    const width = +svg.attr("width") - margin.left - margin.right;
    const height = +svg.attr("height") - margin.top - margin.bottom;
    const chart = svg
      .append("g")
      .attr("transform", `translate(${margin.left},0)`);
    const grp = chart
      .append("g")
      .attr("transform", `translate(-${margin.left},-${margin.top})`);

    const yScale = d3
      .scaleLinear()
      .range([height, 0])
      .domain([0, d3.max(data, dataPoint => dataPoint.count)]);

    const xScale = d3
      .scaleLinear()
      .range([0, width])
      .domain(d3.extent(data, dataPoint => dataPoint.year));

    const line = d3
      .line()
      .x(dataPoint => xScale(dataPoint.year))
      .y(dataPoint => yScale(dataPoint.count));

    // Add path
    grp
      .append("path")
      .attr("transform", `translate(${margin.left},0)`)
      .datum(data)
      .attr("fill", "none")
      .attr("stroke", "steelblue")
      .attr("stroke-linejoin", "round")
      .attr("stroke-linecap", "round")
      .attr("stroke-width", 1.5)
      .attr("d", line);

    // Add the X Axis
    chart
      .append("g")
      .attr("transform", `translate(0,${height})`)
      .call(d3.axisBottom(xScale).ticks(data.length));

    // Add the Y Axis
    chart
      .append("g")
      .attr("transform", `translate(0, 0)`)
      .call(d3.axisLeft(yScale));
  }
};
</script>
