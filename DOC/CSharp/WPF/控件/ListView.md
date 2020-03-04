# ListView

```
<ListView x:Name="DataLv">
    <ListView.View>
        <GridView>
            <GridViewColumn Header="姓名" Width="150" x:Name="ColName">
                <GridViewColumn.CellTemplate>
                    <DataTemplate>
                        <TextBox Width="{Binding ElementName=ColName, Path=Width}"  Text="{Binding name}"/>
                    </DataTemplate>
                </GridViewColumn.CellTemplate>
            </GridViewColumn>
            <GridViewColumn Header="性别" Width="150" x:Name="ColSex">
                <GridViewColumn.CellTemplate>
                    <DataTemplate>
                        <ComboBox Width="100" ItemsSource="{Binding sex}" SelectedIndex="{Binding sexSel}" />
                    </DataTemplate>
                </GridViewColumn.CellTemplate>
            </GridViewColumn>
            <GridViewColumn Header="页面" Width="150" x:Name="ColDataPage">
                <GridViewColumn.CellTemplate>
                    <DataTemplate>
                        <ContentPresenter  Content="{Binding Content}"/>
                    </DataTemplate>
                </GridViewColumn.CellTemplate>
            </GridViewColumn>
        </GridView>
    </ListView.View>
</ListView>



class DataItem
{
    public string name { get; set; }
    public List<string> sex { get; set; }
    public int sexSel { get; set; }

    public UserControl Content { get; set; }
}


List<DataItem> m_DataItem = new List<DataItem>();
DataItem it = new DataItem();
it.name = "aa";
it.sex = new List<string>() { "man","woman"};
it.sexSel = 0;
it.Content = new UC();
m_DataItem.Add(it);
DataLv.ItemsSource = m_DataItem;
```
