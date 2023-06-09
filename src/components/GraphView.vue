<template>
    <div>
      <h2>Graph View</h2>
      <div ref="chartContainer"></div>
    </div>
</template>
  
  <script>
  import { onMounted, ref } from 'vue';
  import axios from 'axios';
  import * as d3 from 'd3';
  
  export default {
    name: 'GraphView',
    setup() {
      const chartData = [];
      const chartContainer = ref(null);
      
  
      const renderChart = () => {
        if (!chartContainer.value) {
          return;
        }
  
        const margin = { top: 20, right: 20, bottom: 30, left: 40 };
        const width = 600 - margin.left - margin.right;
        const height = 400 - margin.top - margin.bottom;
  
        const svg = d3
          .select(chartContainer.value)
          .append('svg')
          .attr('width', width + margin.left + margin.right)
          .attr('height', height + margin.top + margin.bottom)
          .append('g')
          .attr('transform', `translate(${margin.left}, ${margin.top})`);
  
        const x = d3
          .scaleBand()
          .range([0, width])
          .padding(0.1)
          .domain(chartData.map((d) => d.name));
  
        const y = d3.scaleLinear().range([height, 0]).domain([0, d3.max(chartData, (d) => d.value)]);
  
        svg
          .append('g')
          .attr('transform', `translate(0, ${height})`)
          .call(d3.axisBottom(x))
          .selectAll('text')
          .attr('transform', 'rotate(-45)')
          .attr('text-anchor', 'end')
          .attr('dx', '-.8em')
          .attr('dy', '.15em');
  
        svg.append('g').call(d3.axisLeft(y));
  
        svg
          .selectAll('.bar')
          .data(chartData)
          .enter()
          .append('rect')
          .attr('class', 'bar')
          .attr('x', (d) => x(d.name))
          .attr('y', (d) => y(d.value))
          .attr('width', x.bandwidth())
          .attr('height', (d) => height - y(d.value));
      };
  
      onMounted(() => {
        axios
          .get('dependency.json')
          .then((response) => {
            chartData.push(...response.data);
            renderChart();
          })
          .catch((error) => {
            console.error(error);
          });
      });
  
      return { chartContainer };
    },
  };
  </script>
  
  <style scoped>
  h2 {
    font-size: 2rem;
    margin-bottom: 1rem;
  }
  
  svg {
    background-color: #f5f5f5;
    border-radius: 0.25rem;
  }
  </style>
  