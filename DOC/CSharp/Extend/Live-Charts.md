# Live-Charts

```
xmlns:lvc="clr-namespace:LiveCharts.Wpf;assembly=LiveCharts.Wpf"

 <lvc:CartesianChart Series="{Binding SeriesCollection}" LegendLocation="Right" >
     <lvc:CartesianChart.AxisY>
         <lvc:Axis Title="" LabelFormatter="{Binding YFormatter}"></lvc:Axis>
     </lvc:CartesianChart.AxisY>
     <lvc:CartesianChart.AxisX>
         <lvc:Axis Title="" Labels="{Binding Labels}"></lvc:Axis>
     </lvc:CartesianChart.AxisX>
 </lvc:CartesianChart>



ChartValues<double> vv = new ChartValues<double>();
public SeriesCollection SeriesCollection { get; set; }
public string[] Labels { get; set; }
public Func<double, string> YFormatter { get; set; }

YFormatter = value => value + "km";
SeriesCollection = new SeriesCollection();
LineSeries ss = new LineSeries();
ss.Title = title;
ss.LineSmoothness = 0;
ss.Values = vv;
SeriesCollection.Add(ss);
DataContext = this;

List<string> temp = new List<string>();
foreach (var i in ls)
{
    vv.Add(i.y);
    temp.Add(i.x);
}
Labels = temp.ToArray();
```