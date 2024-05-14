import * as d3 from "https://esm.sh/d3";


const w = 1300;
const h = 850;
const padding = 60;
const margin = { top: 40, right: 160, bottom: 60, left: 160 };
const width = w - margin.left - margin.right;
const height = h - margin.top - margin.bottom;

const url = 'https://api.exchangerate-api.com/v4/latest/USD';

fetch(url)
  .then(response => {
    if (!response.ok) {
      throw new Error('Network Error');
    }
    return response.json();
  })
  .then(data => {
    const currencyData = data.rates;

    const tooltip = d3
      .select('#container')
      .append('div')
      .attr('id', 'tooltip')
      .style('opacity', 0);

    const scaleGraphAndPlotData = (data, chartType) => {
      // Sorting currencies 
      const ds = Object.entries(data)
        .sort(([aCurrency, aValue], [bCurrency, bValue]) => bValue - aValue)
        .map(([currency, value]) => ({ currency, value }));

      const svg = d3
        .select('#container')
        .append('svg')
        .attr('width', width + margin.left + margin.right)
        .attr('height', height + margin.top + margin.bottom)
        .append('g')
        .attr('transform', `translate(${margin.left}, ${margin.top})`);

      const yMax = d3.max(ds, (d) => d.value);
      const yScale = d3.scaleLog()
        .domain([0.3, yMax])
        .range([height, 0]);

      const xScale = d3.scaleBand()
        .domain(ds.map((d, i) => i))
        .range([0, width])
        .padding(0.1);

      svg.append('g')
        .classed('axis', true)
        .attr('transform', `translate(0, ${height})`)
        .call(d3.axisBottom(xScale).tickValues([]));

      svg.append('g')
        .classed('axis', true)
        .call(d3.axisLeft(yScale))
        .append('text')
        .attr('transform', 'rotate(-90)')
        .attr('y', 6)
        .attr('dy', '-1em')
        .style('text-anchor', 'end');
        //.text('Value')

      svg.append("text")
            .attr("class", "x label")
            .attr("text-anchor", "middle")
            .attr("x", width / 2)
            .attr("y", height + margin.top )
            .style("font-size", "23px") 
            .text("Name of the currency");

            svg.append("text")
                .attr("class", "y label")
                .attr("text-anchor", "middle")
                .attr("transform", "rotate(-90)")
                .attr("x", -height / 2)
                .attr("y", -margin.left + 100)
                .style("font-size", "23px") 
                .text("Value");


      if (chartType === 'line') {
        const line = d3.line()
          .x((d, i) => xScale(i) + xScale.bandwidth() / 2)
          .y(d => yScale(d.value));

        svg.append('path')
          .datum(ds)
          .attr('fill', 'none')
          .attr('stroke', 'steelblue')
          .attr('stroke-width', 2)
          .attr('d', line);
      } else if (chartType === 'bar') {
        const bars = svg
          .selectAll('.bar')
          .data(ds)
          .enter()
          .append('rect')
          .classed('bar', true)
          .attr('x', (d, i) => xScale(i))
          .attr('width', xScale.bandwidth())
          .attr('height', 0)
          .attr('y', height)
          .on('mouseover', (e, d) => {
            const gdp = d3.format(',.1f')(d.value);
            tooltip
              .transition()
              .duration(500)
              .style('opacity', 0.75)
              .attr('data-currency', d.currency);
            tooltip.html(`${d.currency}: ${gdp}`);
          })
          .on('mouseout', () =>
            tooltip
              .transition()
              .duration(500)
              .style('opacity', 0)
              .attr('data-currency', '')
          )
          .on('mousemove', (e) =>
            tooltip.style('left', (e.pageX + 20) + 'px').style('top', (e.pageY - 20) + 'px')
          );

        bars
          .transition()
          .duration(2500)
          .attr('y', (d) => yScale(d.value))
          .attr('height', (d) => height - yScale(d.value));
      } else if (chartType === 'scatter') {
        svg.selectAll(".dot")
          .data(ds)
          .enter().append("circle")
          .attr("class", "dot")
          .attr("cx", (d, i) => xScale(i) + xScale.bandwidth() / 2)
          .attr("cy", (d) => yScale(d.value))
          .attr("r", 5)
          .style("fill", "steelblue")
          .on('mouseover', (e, d) => {
            const gdp = d3.format(',.1f')(d.value);
            tooltip
              .transition()
              .duration(500)
              .style('opacity', 0.75)
              .attr('data-currency', d.currency);
            tooltip.html(`${d.currency}: ${gdp}`);
          })
          .on('mouseout', () =>
            tooltip
              .transition()
              .duration(500)
              .style('opacity', 0)
              .attr('data-currency', '')
          )
          .on('mousemove', (e) =>
            tooltip.style('left', (e.pageX + 20) + 'px').style('top', (e.pageY - 20) + 'px')
          );
      }
    };

    const select = document.getElementById('chartTypeSelect');
    select.addEventListener('change', (event) => {
      const chartType = event.target.value;
      d3.select('svg').remove(); 
      scaleGraphAndPlotData(currencyData, chartType);
    });

    const defaultChartType = 'bar';
    scaleGraphAndPlotData(currencyData, defaultChartType);
  })
  .catch(error => console.error('Error fetching data:', error));
