<template>
    <div id="data1"></div>
</template>
<script>
import * as d3 from 'd3';
export default {
    mounted(){
 const headers = { "Content-Type": "application/json" };
    fetch("z.json", { headers })
      .then((response) => response.json())
      .then((data) => {
        console.log(data);

        const margin={top:20,bottom:20,left:20,right:20},
        width=460-margin.left-margin.right,
        height=400-margin.top-margin.bottom;

        const svg=d3.select("#data1")
        .append("svg")
        .attr("width",width+margin.left+margin.right)
        .attr("height",height+margin.top+margin.bottom)
        .append("g")
        .attr("transform",
        "translate("+margin.left+","+margin.top+")");

        const xScale=d3.scaleBand()
        .range([0,width])
        .padding(0.4); 
        const yScale=d3.scaleLinear()
        .range([height,0]);



xScale.domain(data.map(function(d) { return d.year; }));
        yScale.domain([0, d3.max(data, function(d) { return d.value; })]);

        svg.append("g")
         .attr("transform", "translate(0," + height + ")")
         .call(d3.axisBottom(xScale));

        svg.append("g")
         .call(d3.axisLeft(yScale).tickFormat(function(d){
             return "$" + d;
         }).ticks(10));


        svg.selectAll(".bar")
         .data(data)
         .enter().append("rect")
         .attr("class", "bar")
         .attr("x", function(d) { return xScale(d.year); })
         .attr("y", function(d) { return yScale(d.value); })
         .attr("width", xScale.bandwidth())
         .attr("height", function(d) { return height - yScale(d.value); });

    })
    }
}
</script>
<style scoped>

</style>