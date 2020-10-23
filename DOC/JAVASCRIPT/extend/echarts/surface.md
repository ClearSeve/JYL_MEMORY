# surface

```
echarts-gl.min.js

//data.push([x1, y1, z]);
//data.push([x2, y1, z]);
//data.push([x3, y1, z]);
//data.push([x1, y2, z]);
//data.push([x2, y2, z]);
//data.push([x3, y2, z]);
var data = [
    [-4,-4,-0.58617619],
    [-2,-4,-0.9712778],
    [0,-4,-0.7568025],
    [2,-4,-0.9712778],


    [-4,-2,-0.9712778],
    [-2,-2,0.30807174],
    [0,-2,0.90929743],
    [2,-2,0.30807174],



    [-4, 0,-0.7568025],
    [-2, 0,0.90929743],
    [0, 0,0.30807174],
    [2, 0,0.90929743],

    [-4, 2,-0.9712778],
    [-2, 2,0.30807174],
    [0, 2,0.90929743],
    [2, 2,0.30807174],
];


var option = {
    tooltip: {},
    backgroundColor: '#fff',
    visualMap: {
        show: false,
        dimension: 2,
        min: -1,
        max: 1,
        inRange: {
            color: ['#313695', '#4575b4', '#74add1', '#abd9e9', '#e0f3f8', '#ffffbf', '#fee090', '#fdae61', '#f46d43', '#d73027', '#a50026']
        }
    },
    xAxis3D: {
        type: 'value'
    },
    yAxis3D: {
        type: 'value'
    },
    zAxis3D: {
        type: 'value'
    },
    grid3D: {
        viewControl: {
            projection: 'orthographic'
        }
    },
    series: [{
        type: 'surface',
        data: data
    }]
};

```