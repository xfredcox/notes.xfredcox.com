## Reusable Architecture
 
The two main considerations for implementing reusable charts are:

- Implement charts as closures with getter/setter methods for attribute variables
- The closure takes a selection as an arguments, allowing us to use the chart as a "stamp" on DOM elements.

Here is an example time series:

``javascript
function timeSeriesChart() {
  // Default values
  var margin = {top: 20, right: 20, bottom: 20, left: 20},
      width = 760,
      height = 120,
      xValue = function(d) { return d[0]; },
      yValue = function(d) { return d[1]; },
      xScale = d3.time.scale(),
      yScale = d3.scale.linear(),
      xAxis = d3.svg.axis().scale(xScale).orient("bottom").tickSize(6, 0),
      area = d3.svg.area().x(X).y1(Y),
      line = d3.svg.line().x(X).y(Y);
  //
  // Closure function takes a selection
  function chart(selection) {
    selection.each(function(data) {
      // Convert data to standard representation greedily;
      // this is needed for nondeterministic accessors.
      data = data.map(function(d, i) {
        return [xValue.call(data, d, i), yValue.call(data, d, i)];
      });
      // Update the x-scale.
      xScale
          .domain(d3.extent(data, function(d) { return d[0]; }))
          .range([0, width - margin.left - margin.right]);
      // Update the y-scale.
      yScale
          .domain([0, d3.max(data, function(d) { return d[1]; })])
          .range([height - margin.top - margin.bottom, 0]);
      // 
      // Select the svg element, if it exists. 
      // Note the initial selection "this".
      var svg = d3.select(this).selectAll("svg").data([data]);
      // Otherwise, create the skeletal chart.
      var gEnter = svg.enter().append("svg").append("g");
      gEnter.append("path").attr("class", "area");
      gEnter.append("path").attr("class", "line");
      gEnter.append("g").attr("class", "x axis");
      // Update the outer dimensions.
      svg .attr("width", width)
          .attr("height", height);
      // Update the inner dimensions.
      var g = svg.select("g")
          .attr("transform", "translate(" + margin.left + "," + margin.top + ")");
      // Update the area path.
      g.select(".area")
          .attr("d", area.y0(yScale.range()[0]));
      // Update the line path.
      g.select(".line")
          .attr("d", line);
      // Update the x-axis.
      g.select(".x.axis")
          .attr("transform", "translate(0," + yScale.range()[0] + ")")
          .call(xAxis);
    });
  }
  // The x-accessor for the path generator; maps xScale to xValue.
  function X(d) {
    return xScale(d[0]);
  }
  // The x-accessor for the path generator; maps yScale to yValue.
  function Y(d) {
    return yScale(d[1]);
  }
  // .margin(value) getter/setter
  chart.margin = function(_) {
    if (!arguments.length) return margin;
    margin = _;
    return chart;
  };
  // .width(value) getter/setter
  chart.width = function(_) {
    if (!arguments.length) return width;
    width = _;
    return chart;
  };
  // .height(value) getter/setter
  chart.height = function(_) {
    if (!arguments.length) return height;
    height = _;
    return chart;
  };
  // .x(value) getter/setter
  chart.x = function(_) {
    if (!arguments.length) return xValue;
    xValue = _;
    return chart;
  };
  // .y(value) getter/setter
  chart.y = function(_) {
    if (!arguments.length) return yValue;
    yValue = _;
    return chart;
  };
  // Returns the chart instance.
  return chart;
}
``