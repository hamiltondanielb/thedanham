---
title: Creating the Graphs as React Components
date: "2017-03-14T22:12:03.284Z"
description: "Creating the Graphs as React Components"
---

Because of the complexity of [D3.js](https://d3js.org/), I am going to break these tutorials into two articles.

There are quite a bit of articles out there on getting started with D3.js. In this article, I will assume you have some basic understanding of D3, and will focus on using D3 as a React component.

### Creating the Bars
In D3, creating a bar for a bar graph is pretty easy — it is just a group of SVG rectangles drawn to the screen. First, we want create a simple component to allow us to configure individual bars. The `props` allow us to change the color, width, height, and location of each bar. I will get into how we can use these `props` to display data later.

```javascript
import React from 'react';

const Bar = (props) => {
  return (
    <rect
      className={props.className}
      {...props}
    />
  );
}

Bar.propTypes = {
  fill: React.PropTypes.string,
  width: React.PropTypes.number,
  height: React.PropTypes.number,
  x: React.PropTypes.number,
  y: React.PropTypes.number,
  className: React.PropTypes.string
}

Bar.defaultProps = { className: 'barchart-bar' };

export default Bar;
```

### Creating the Line
In D3, you create a path to plot data points on the screen, and then draw a line between the points. Same with the bars, by allowing the passing in of `props`, we can plot the points and line for the path.

```javascript
import React from 'react';

const Path = (props) => {
  return (
    <path
      className={props.class}
      d={props.path}
      {...props} />
  );
}

Path.propTypes = {
  fill: React.PropTypes.string,
  path: React.PropTypes.string,
  stroke: React.PropTypes.string,
  strokeWidth: React.PropTypes.number,
  className: React.PropTypes.string
}

Path.defaultProps = { className: 'linechart-path' };

export default Path;
```

### Creating the BarLine Graph
I created a component that simply takes in data via props and combines the previous two components together. Putting them together like this allows us to scale the bars and lines together. The D3 code here can be pretty complex, but take a look at Egghead.io's D3 course (and I will have a intro to D3 post soon). To summerize, we are calculating the scale of the bars and lines (I am skipping the scaling of the axis, but it is similar to scaling the bars and path).

```javascript
import * as d3 from 'd3'
import React from 'react'
import Chart from '../chart'
import Axis from '../axis'
import DataSeries from './DataSeries'

class BarLineChart extends React.Component {

  render () {
    return (
      <Chart
        width={this.props.width}
        height={this.props.height + this.props.margins.top + this.props.margins.bottom}>
          <g
            transform={`translate(${this.props.margins.left}, 	 ${this.props.margins.top})`}>
            <DataSeries
              height={this.props.height}
              width={this.props.height}
              lineData={this.props.linedata}
              barData={this.props.data}
            />
          </g>
      </Chart>
    )
  }
}

BarLineChart.propTypes = {
  margins: React.PropTypes.obj,
  barWidth: React.PropTypes.number,
  barMargin: React.PropTypes.number,
  width: React.PropTypes.number,
  height: React.PropTypes.number,
  className: React.PropTypes.string,
}

BarLineChart.defaultProps = {
  className: 'BarLineChart',
  margins: {top: 20, right: 20, bottom: 30, left: 40},
  barWidth: 15,
  barMargin: 15,
}

export default BarLineChart

```

Here is the final product:

![Screenshot 2016-09-06 13.40.21.png](https://cdn.filestackcontent.com/BCo7rVmR52i1WW3bDsoq)
