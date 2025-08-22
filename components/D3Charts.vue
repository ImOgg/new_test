<template>
    <div>
      <h2>分組長條圖：商場 A 和 B 的分類銷售數據比較</h2>
      <div ref="groupedBarChart"></div>
  
      <h2>折線圖：商場 A 和 B 的分類銷售趨勢</h2>
      <div ref="lineChart"></div>
  
      <h2>圓餅圖：商場 A 和 B 的銷售比例</h2>
      <div>
        <div>
          <h3>商場 A</h3>
          <div ref="pieChartA"></div>
        </div>
        <div>
          <h3>商場 B</h3>
          <div ref="pieChartB"></div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import * as d3 from 'd3';
  import { ref, onMounted } from 'vue';
  
  // 資料結構
  const data = [
    { mall: '商場A', category: '食品', value: 400 },
    { mall: '商場A', category: '電子產品', value: 300 },
    { mall: '商場A', category: '衣物', value: 200 },
    { mall: '商場A', category: '家具', value: 100 },
    { mall: '商場B', category: '食品', value: 350 },
    { mall: '商場B', category: '電子產品', value: 280 },
    { mall: '商場B', category: '衣物', value: 220 },
    { mall: '商場B', category: '家具', value: 150 },
  ];
  
  // 圖表容器
  const groupedBarChart = ref<HTMLDivElement | null>(null);
  const lineChart = ref<HTMLDivElement | null>(null);
  const pieChartA = ref<HTMLDivElement | null>(null);
  const pieChartB = ref<HTMLDivElement | null>(null);
  
  // 分組長條圖
  const createGroupedBarChart = (data: typeof data) => {
    const width = 600;
    const height = 400;
    const margin = { top: 20, right: 30, bottom: 50, left: 40 };
  
    const svg = d3
      .select(groupedBarChart.value)
      .append('svg')
      .attr('width', width)
      .attr('height', height);
  
    const categories = Array.from(new Set(data.map((d) => d.category)));
    const malls = Array.from(new Set(data.map((d) => d.mall)));
  
    const x0 = d3.scaleBand().domain(categories).range([margin.left, width - margin.right]).padding(0.2);
    const x1 = d3.scaleBand().domain(malls).range([0, x0.bandwidth()]).padding(0.05);
    const y = d3.scaleLinear().domain([0, d3.max(data, (d) => d.value)!]).nice().range([height - margin.bottom, margin.top]);
    const color = d3.scaleOrdinal<string>().domain(malls).range(['steelblue', 'orange']);
  
    svg.append('g').attr('transform', `translate(0,${height - margin.bottom})`).call(d3.axisBottom(x0));
    svg.append('g').attr('transform', `translate(${margin.left},0)`).call(d3.axisLeft(y));
  
    svg
      .append('g')
      .selectAll('g')
      .data(d3.group(data, (d) => d.category))
      .join('g')
      .attr('transform', (d) => `translate(${x0(d[0])},0)`)
      .selectAll('rect')
      .data((d) => d[1])
      .join('rect')
      .attr('x', (d) => x1(d.mall)!)
      .attr('y', (d) => y(d.value))
      .attr('height', (d) => y(0) - y(d.value))
      .attr('width', x1.bandwidth())
      .attr('fill', (d) => color(d.mall)!);
  };
  
  // 折線圖
  const createLineChart = (data: typeof data) => {
    const width = 600;
    const height = 400;
    const margin = { top: 20, right: 30, bottom: 50, left: 40 };
  
    const svg = d3
      .select(lineChart.value)
      .append('svg')
      .attr('width', width)
      .attr('height', height);
  
    const categories = Array.from(new Set(data.map((d) => d.category)));
    const malls = Array.from(new Set(data.map((d) => d.mall)));
  
    const x = d3.scalePoint().domain(categories).range([margin.left, width - margin.right]);
    const y = d3.scaleLinear().domain([0, d3.max(data, (d) => d.value)!]).nice().range([height - margin.bottom, margin.top]);
    const color = d3.scaleOrdinal<string>().domain(malls).range(['steelblue', 'orange']);
  
    svg.append('g').attr('transform', `translate(0,${height - margin.bottom})`).call(d3.axisBottom(x));
    svg.append('g').attr('transform', `translate(${margin.left},0)`).call(d3.axisLeft(y));
  
    const groupedData = d3.group(data, (d) => d.mall);
  
    groupedData.forEach((values, mall) => {
      svg
        .append('path')
        .datum(values)
        .attr('fill', 'none')
        .attr('stroke', color(mall)!)
        .attr('stroke-width', 2)
        .attr(
          'd',
          d3
            .line<typeof values[0]>()
            .x((d) => x(d.category)!)
            .y((d) => y(d.value))
            .curve(d3.curveMonotoneX)
        );
  
      svg
        .selectAll('circle')
        .data(values)
        .join('circle')
        .attr('cx', (d) => x(d.category)!)
        .attr('cy', (d) => y(d.value))
        .attr('r', 5)
        .attr('fill', color(mall)!);
    });
  };
  
  // 圓餅圖
  const createPieChart = (data: typeof data, ref: HTMLDivElement | null) => {
    const width = 300;
    const height = 300;
    const radius = Math.min(width, height) / 2;
  
    const svg = d3
      .select(ref)
      .append('svg')
      .attr('width', width)
      .attr('height', height)
      .append('g')
      .attr('transform', `translate(${width / 2}, ${height / 2})`);
  
    const pie = d3.pie<typeof data[0]>().value((d) => d.value);
    const arc = d3.arc<d3.PieArcDatum<typeof data[0]>>().innerRadius(0).outerRadius(radius);
    const color = d3.scaleOrdinal<string>().domain(data.map((d) => d.category)).range(d3.schemeCategory10);
  
    svg
      .selectAll('path')
      .data(pie(data))
      .join('path')
      .attr('d', arc)
      .attr('fill', (d) => color(d.data.category)!);
  };
  
  onMounted(() => {
    createGroupedBarChart(data);
    createLineChart(data);
  
    const dataA = data.filter((d) => d.mall === '商場A');
    const dataB = data.filter((d) => d.mall === '商場B');
  
    createPieChart(dataA, pieChartA.value);
    createPieChart(dataB, pieChartB.value);
  });
  </script>
  
  <style>
  /* 圖表容器的樣式 */
  div {
    margin-bottom: 40px;
  }
  </style>
  