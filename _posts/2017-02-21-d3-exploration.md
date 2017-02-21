---
layout: post
title: "Exploring d3"
categories:
  - Metis
  - d3
tags:
  - Metis

---
For those of you interested in boot camps and switching fields, here's a bit about where I'm coming from and how Metis has been so far. 

### Holy
Testing d3:
<head>
  <meta charset="utf-8">

  <style>
  .names {
    fill: none;
    stroke: #fff;
    stroke-linejoin: round;
  }
  
  /* Tooltip CSS */
  .d3-tip {
    line-height: 1.5;
    font-weight: 400;
    font-family:"avenir next", Arial, sans-serif;
    padding: 6px;
    background: rgba(0, 0, 0, 0.6);
    color: #FFA500;
    border-radius: 1px;
    pointer-events: none;
  }

  /* Creates a small triangle extender for the tooltip */
  .d3-tip:after {      
    box-sizing: border-box;
    display: inline;
    font-size: 8px;
    width: 100%;
    line-height: 1.5;
    color: rgba(0, 0, 0, 0.6);
    position: absolute;
    pointer-events: none;
    
  }

  /* Northward tooltips */
  .d3-tip.n:after {
    content: "\25BC";
    margin: -1px 0 0 0;
    top: 100%;
    left: 0;
    text-align: center;
  }

  /* Eastward tooltips */
  .d3-tip.e:after {
    content: "\25C0";
    margin: -4px 0 0 0;
    top: 50%;
    left: -8px;
  }

  /* Southward tooltips */
  .d3-tip.s:after {
    content: "\25B2";
    margin: 0 0 1px 0;
    top: -8px;
    left: 0;
    text-align: center;
  }

  /* Westward tooltips */
  .d3-tip.w:after {
    content: "\25B6";
    margin: -4px 0 0 -1px;
    top: 50%;
    left: 100%;
  }

  /*    
  text{
    pointer-events:none;
  }
  */

  .details{
    color: white;
  }
  </style>
  <title></title>


</head>
<body>
<script src="https://d3js.org/d3.v4.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/d3-legend/2.21.0/d3-legend.js"></script>
<script src="https://d3js.org/queue.v1.min.js"></script>
<script src="https://d3js.org/topojson.v1.min.js"></script>
<script src="https://d3js.org/d3-geo-projection.v1.min.js"></script>
<script src="{{ site.url }}/assets/js/d3-tip.js"></script>
<script src="{{ site.url }}/assets/js/jenks.js"></script>
<script src='https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/6.10.3/babel.min.js'></script>

<script lang='babel' type='text/babel'>
// configuration
const colorVariable = 'population';
const geoIDVariable = 'id';
const format = d3.format(',');

// Set tooltips
const tip = d3.tip()
  .attr('class', 'd3-tip')
  .offset([-10, 0])
  .html(d => `<strong>Country: </strong><span class='details'>${d.properties.name}<br></span>
  <strong>Species: </strong><span class='details'>${d.speciesCount}</span>`);

const margin = {top: 5, right: 100, bottom: 5, left: 5};
width 960 -  is standard...
const width = 960 - margin.left - margin.right;
const height = 500 - margin.top - margin.bottom;

const color = d3.scaleQuantile()
      .range([
    'rgb(7,79,151)',
    'rgb(17,119,220)',
    'rgb(82,165,249)',
    'rgb(181,218,255)', 
    'rgb(236,237,255)',
    'rgb(250,192,194)', 
    'rgb(253,108,113)', 
    'rgb(228,43,49)',
    'rgb(186,7,13)',
    'rgb(138,5,9)'
  ]);


const svg = d3.select('#chart')
  .append('svg')
  .attr('width', width + margin.left + margin.right)
  .attr('height', height + margin.top + margin.bottom)
  .append('g')
  .attr('class', 'map')
  .attr('transform', 'translate(' + [margin.left, margin.top] + ')');

const projection = d3.geoRobinson()
  .scale(148)
  .rotate([352, 0, 0])
  .translate( [width / 2, height / 2]);


const path = d3.geoPath().projection(projection);

svg.call(tip);

//{{ site.url }}/images/project_be
queue()
  .defer(d3.json, '{{ site.url }}/d3data/world_countries.json')
  .defer(d3.csv, '{{ site.url }}/d3data/world_mean.csv')
  .defer(d3.csv, '{{ site.url }}/d3data/species_counts.csv')
  .await(ready);

//queue()
//  .defer(d3.json, '{{url_for('static', filename ='world_countries.json')}}')
//  .defer(d3.csv, '{{url_for('static', filename ='world_mean.csv')}}')
//  .defer(d3.csv, '{{url_for('static', filename ='species_counts.csv')}}')
//  .await(ready);

function ready(error, geography, data, speciesCounts) {
  data.forEach(d => {
    d[colorVariable] = Number(d[colorVariable].replace(',', ''));
  });

  var map = d3.map([{name: "foo"}, {name: "bar"}], function(d) { return d.name; });
  map.get("foo"); // {"name": "foo"}

  const speciesCountbyID = {};

  console.log(speciesCounts);
  speciesCounts.forEach(d => { 
    speciesCountbyID[d[geoIDVariable]] = d["counts"]; 
  });
  console.log(speciesCountbyID);


  const colorVariableValueByID = {};

  data.forEach(d => { colorVariableValueByID[d[geoIDVariable]] = d[colorVariable]; });
  geography.features.forEach(d => { d[colorVariable] = colorVariableValueByID[d.id] });

  // calculate jenks natural breaks
  const numberOfClasses = color.range().length - 1;
  const jenksNaturalBreaks = jenks(data.map(d => d[colorVariable]), numberOfClasses);
  console.log('numberOfClasses', numberOfClasses);
  console.log('jenksNaturalBreaks', jenksNaturalBreaks);

  // set the domain of the color scale based on our data
  color
    .domain(jenksNaturalBreaks);
  console.log(jenksNaturalBreaks);
  console.log("range", d3.extent(jenksNaturalBreaks))


  svg.append('g')
    .attr('class', 'countries')
    .selectAll('path')
    .data(geography.features)
    .enter().append('path')
      .attr('d', path)
      .style('fill', d => {
        if (typeof colorVariableValueByID[d.id] !== 'undefined') {
          return color(colorVariableValueByID[d.id])
        } 
        return 'white'
      })
      .style('fill-opacity',0.8)
      .style('stroke', d => {
          if (d[colorVariable] !== 0) {
          return 'white';
        } 
        return 'lightgray';
      })
      .style('stroke-width', 1)
      .style('stroke-opacity', 0.5)
      // tooltips
      .on('mouseover',function(d){
        d.speciesCount = speciesCountbyID[d[geoIDVariable]];
        tip.show(d);
        d3.select(this)
          .style('fill-opacity', 1)
          .style('stroke-opacity', 1)
          .style('stroke-width', 2)
      })
      .on('mouseout', function(d){
        tip.hide(d);
        d3.select(this)
          .style('fill-opacity', 0.8)
          .style('stroke-opacity', 0.5)
          .style('stroke-width', 1)
      });

  svg.append('path')
    .datum(topojson.mesh(geography.features, (a, b) => a.id !== b.id))
    .attr('class', 'names')
    .attr('d', path);

svg.append("g")
  .attr("class", "legendLinear")
  .attr("transform", "translate(" + [width,50] + ")"); //location

const legendFormat = d3.format(".3f");
const legendLabels = jenksNaturalBreaks.map(d => legendFormat(d));
console.log(legendLabels);

var legendLinear = d3.legendColor()
  .shapeWidth(30)
  .cells(jenksNaturalBreaks)
  .labels(legendLabels)
  .title("Mean IUCN Status Change")
  .titleWidth(100)
  .orient('vertical')
  .scale(color);

svg.select(".legendLinear")
  .call(legendLinear);

d3.select(window).on('resize', resize);

}
</script>

</body>




