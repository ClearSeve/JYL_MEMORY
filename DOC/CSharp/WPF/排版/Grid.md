# Grid

***.SetValue(Grid.ColumnProperty, 2);

```
<Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="3*"/>
            <RowDefinition Height="2*"/>
        </Grid.RowDefinitions>
        <Grid Grid.Row="0">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="200" />
                <ColumnDefinition />
                <ColumnDefinition Width="200" />
            </Grid.ColumnDefinitions>
            <Button Content="btn1" Grid.Column="0" HorizontalAlignment="Left" Margin="48,60,0,0" VerticalAlignment="Top" Width="75"/>
            <Button Content="btn2" Grid.Column="2" HorizontalAlignment="Left" Margin="60,60,0,0" VerticalAlignment="Top" Width="75"/>
        </Grid>
        <Grid Grid.Row="1">
            <Grid.RowDefinitions>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition />
                <ColumnDefinition />
                <ColumnDefinition />
            </Grid.ColumnDefinitions>
            <Border Grid.Row="0" Grid.Column="1">
                <Button Content="btn3" />
            </Border>
        </Grid>
</Grid>
```
