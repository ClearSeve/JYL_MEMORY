# OxyPlot

```
 xmlns:oxy="http://oxyplot.org/wpf"

 <oxy:PlotView  Model="{Binding Path= MyPlotModel}"></oxy:PlotView>
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

        MyPlotModel.MouseDown += (object sender, OxyMouseDownEventArgs e) =>
        {
            var position = e.Position;
            var jlSeries = (OxyPlot.Series.LineSeries)MyPlotModel.Series[0];
            ScreenPoint screenPoint = new ScreenPoint(position.X, position.Y);
            var datapoint = jlSeries.InverseTransform(screenPoint);
            var x = Math.Round(datapoint.X);
            System.Diagnostics.Debug.WriteLine(x);
        };
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


    public void SetYRange(double max,double min)
    {
        line1.YAxis.Maximum = max;
        line1.YAxis.Minimum = min;
    }
    void DiyAxis()
    {
         OxyColor clr = OxyColor.FromRgb(198, 201, 206);
            //定义y轴
            LinearAxis leftAxis = new LinearAxis()
            {
                Position = AxisPosition.Left,
                Minimum = 0,
                Maximum = 100,
                Title = "y轴标题",
                TitlePosition = 0,
                MajorGridlineStyle = LineStyle.Solid,
                MinorGridlineStyle = LineStyle.None,
                TextColor = clr,
            };

            //定义x轴
            DateTimeAxis bottomAxis = new DateTimeAxis()
            {
                Position = AxisPosition.Bottom,
                StringFormat = "hh:mm:ss",
                //Minimum = DateTimeAxis.ToDouble(DateTime.Now),
                //Maximum = DateTimeAxis.ToDouble(DateTime.Now.AddMinutes(1)),
                Title = "时间",
                TitlePosition = 0,
                IntervalLength = 60,
                MinorIntervalType = DateTimeIntervalType.Seconds,
                IntervalType = DateTimeIntervalType.Seconds,
                MajorGridlineStyle = LineStyle.Solid,
                MinorGridlineStyle = LineStyle.None,
            };
            MyPlotModel.Axes.Add(leftAxis);
            MyPlotModel.Axes.Add(bottomAxis);
    }
}
```

```
PlotModelCtl m_PlotModelCtl = new PlotModelCtl();
this.DataContext = m_PlotModelCtl;
```