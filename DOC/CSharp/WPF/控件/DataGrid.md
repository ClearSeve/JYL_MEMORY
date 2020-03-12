# DataGrid

## 数据绑定

### combox相关资源定义

```
xmlns:core="clr-namespace:System;assembly=mscorlib"

    <Window.Resources>
        <ObjectDataProvider x:Key="SexEnumKey" MethodName="GetValues"  ObjectType="{x:Type core:Enum}">
            <ObjectDataProvider.MethodParameters>
                <x:Type Type="local:SexEnum"/>
            </ObjectDataProvider.MethodParameters>
        </ObjectDataProvider>
</Window.Resources>
```

### 前台界面

```
<DataGrid Name="DataGrid_Name" ItemsSource="{Binding}" CanUserAddRows="False" AutoGenerateColumns="False" HeadersVisibility="Column">
             <DataGrid.Columns>
                <DataGridTextColumn Header="姓名"  Binding="{Binding Name}"  Width="100"/>
                <DataGridComboBoxColumn Header="性别"  SelectedItemBinding="{Binding Sex}" ItemsSource="{Binding Source={StaticResource SexEnumKey}}"  Width="100"/>
            </DataGrid.Columns>
</DataGrid>
```

### 后台后台数据

```
public enum SexEnum { Male,FeMale};
public class DataItem
{
    public string  Name { get; set; }
    public SexEnum Sex { get; set; }
}


ObservableCollection<DataItem> m_data = new ObservableCollection<DataItem>();

DataItem cm = new DataItem();
cm.Name = "AA";
cm.Sex = SexEnum.Male;
m_data.Add(cm);

DataGrid_Name.DataContext = m_data;
```

## 设置不可编辑

IsReadOnly = true;

## 禁用用户排序

CanUserSortColumns="False"

## 背景色

`<DataGrid Background="#363636" Foreground="White">`

## 修改列名

SourceList.Columns[0].Header = "新标题";

## 单击事件

PreviewMouseLeftButtonDown

## 隐藏表头

HeadersVisibility="Column"

## 表头文字居中

```
<DataGrid.ColumnHeaderStyle>
    <Style TargetType="DataGridColumnHeader">
        <Setter Property="HorizontalContentAlignment" Value="Center" />
    </Style>
</DataGrid.ColumnHeaderStyle>
```

## 选择选择行样式

```
<DataGrid.RowStyle >
    <Style TargetType="DataGridRow">
        <Setter Property="Background" Value="White"/>
        <Style.Triggers>
            <Trigger Property="IsMouseOver" Value="True">
                <Setter Property="Background" Value="LightGray"/>
            </Trigger>
            <Trigger Property="IsSelected" Value="True">
                <Setter Property="Background" Value="#90F670"/>
                <Setter Property="Foreground" Value="White"/>
            </Trigger>
        </Style.Triggers>
    </Style>
</DataGrid.RowStyle>
<DataGrid.Resources>
      <SolidColorBrush x:Key="{x:Static SystemColors.InactiveSelectionHighlightBrushKey}" Color="DodgerBlue"/>
</DataGrid.Resources>
```

## 行跳转

dg.SelectedIndex = 123;  
dg.ScrollIntoView(dg.SelectedItem);  

## 双击获取行数据

MouseDoubleClick  

```
DataGrid dg = sender as DataGrid;
int index = dg.SelectedIndex;
dgItem item = dg.SelectedItem as dgItem;
if (null == item) return;


判断点击位置的列名：
if (dg.CurrentColumn.Header.ToString() != "列名") return;
```

## 获取多行

```
List<PrjDat> itemList = new List<PrjDat>();
foreach(PrjDat item in PrjDatSouce.SelectedItems)
{
    itemList.Add(item);
}

foreach (PrjDat item in itemList)
{
     m_data.Remove(item);
}
```

## 动态表

```
ObservableCollection<ExpandoObject> items = new ObservableCollection<ExpandoObject>();

for (int i = 0; i < 5; i++)
{
    dynamic item = new ExpandoObject();
    item.data1 = i.ToString();
    item.data2 = i.ToString();
    items.Add(item);
}
DbTable.Columns.Add(new DataGridTextColumn() { Header = "数据1", Binding = new Binding("data2") });
DbTable.Columns.Add(new DataGridTextColumn() { Header = "数据2", Binding = new Binding("data2") });
DbTable.ItemsSource = items;
```
