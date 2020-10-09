# OxyPlot

```
 xmlns:oxy="http://oxyplot.org/wpf"

 <oxy:PlotView  Model="{Binding Path= MyPlotModel}" IsEnabled="False"></oxy:PlotView>
```


```
class PlotModelCtl
{
    public PlotModel MyPlotModel { get; set; }
    LineSeries line1 = new LineSeries() { Title = "curve1" };
    LineSeries line2 = new LineSeries() { Title = "curve2" };
    public PlotModelCtl()
    {
        MyPlotModel = new PlotModel();
        MyPlotModel.Series.Add(line1);
        MyPlotModel.Series.Add(line2);
    }
    public void Clear()
    {
        line1.Points.Clear();
        line2.Points.Clear();
    }
    public void Add(int x, double val1, double val2)
    {
        line1.Points.Add(new DataPoint(x, val1));
        line2.Points.Add(new DataPoint(x, val2));
    }
    public void Flush()
    {
        MyPlotModel.InvalidatePlot(true);
    }
}
```

```
PlotModelCtl m_PlotModelCtl = new PlotModelCtl();
this.DataContext = m_PlotModelCtl;
```