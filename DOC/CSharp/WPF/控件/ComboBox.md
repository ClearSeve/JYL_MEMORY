# ComboBox

```
<ComboBox Name="cbb"/>
IsEditable="True" 下拉框可编辑

cbb.Items.Insert(0,"aa");
cbb.Items.Insert(1, "bb");
int index = cbb.SelectedIndex;
```

## 颜色选择

```
<ComboBox  Name="clrCb">
    <ComboBox.ItemTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <Rectangle Fill="{Binding ShowClrName}" Width="100" Height="10"></Rectangle>
                <Rectangle Width="10"></Rectangle>
                <TextBlock Text="{Binding Name}"></TextBlock>
            </StackPanel>
        </DataTemplate>
    </ComboBox.ItemTemplate>
</ComboBox>


class ColorItem
{
    public string ShowClrName { get; set; }
    public string Name { get; set; }
}
clrItem.Add(new ColorItem() { ShowClrName = "Red", Name = "Red" });
clrCb.ItemsSource = clrItem;
```

## 系统颜色选择

```
<controls:MetroWindow.Resources>
    <ObjectDataProvider 
        ObjectInstance="{x:Type Colors}" 
        MethodName="GetProperties" 
        x:Key="colorPropertiesOdp" />
</controls:MetroWindow.Resources>

<ComboBox  ItemsSource="{Binding Source={StaticResource colorPropertiesOdp}}">
    <ComboBox.ItemTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <Rectangle Fill="{Binding Name}" Width="40" Height="10"></Rectangle>
                <TextBlock Text="{Binding Name}"></TextBlock>
            </StackPanel>
        </DataTemplate>
    </ComboBox.ItemTemplate>
</ComboBox>
```
