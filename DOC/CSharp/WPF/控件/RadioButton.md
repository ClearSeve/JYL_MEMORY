# RadioButton

## 使用

```
<StackPanel Orientation="Horizontal">
    <RadioButton  x:Name="A" Content="A"/>
    <RadioButton  x:Name="B" Content="B"/>
</StackPanel>
<StackPanel Orientation="Horizontal">
    <RadioButton  x:Name="C" Content="C"/>
    <RadioButton x:Name="D"   Content="D"/>
</StackPanel>

A.IsChecked = true;
D.IsChecked = true;
放在一个区域内的元素会自动分组
```

## 数据绑定
```
命名空间内定义数据：
public enum EmunStr
{
    XX,
    YY,
}
public class EnumToBooleanConverter : IValueConverter
{
    object IValueConverter.Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return value == null ? false : value.Equals(parameter);
    }
    object IValueConverter.ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return value != null && value.Equals(true) ? parameter : Binding.DoNothing;
    }
}
public class DataModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;
    private void OnPropertyChanged(string p_propertyName)
    {
        PropertyChanged(this, new PropertyChangedEventArgs(p_propertyName));
    }
    private EmunStr _Dat;
    public EmunStr Dat
    {
        get
        {
            return _Dat;
        }
        set
        {
            _Dat = value;
            OnPropertyChanged("Dat");
        }
    }
}
```

```
Xaml的Window中引入资源
 <Window.Resources>
        <local:EnumToBooleanConverter x:Key="EnumToBooleanConverter" />
</Window.Resources>


<RadioButton  Content="A" IsChecked="{Binding Path=Dat,Converter={StaticResource EnumToBooleanConverter},ConverterParameter={x:Static local:EmunStr.XX}}"></RadioButton>
<RadioButton  Content="B" IsChecked="{Binding Path=Dat,Converter={StaticResource EnumToBooleanConverter},ConverterParameter={x:Static local:EmunStr.YY}}"></RadioButton>
<TextBox  Grid.Row="1"  Text="{Binding Path=Dat}"></TextBox>
```

```
DataModel m_dataModel;

m_dataModel = new DataModel();
DataContext = m_dataModel;

获取选择数据：
m_dataModel.SampleEnum.ToString()
```
