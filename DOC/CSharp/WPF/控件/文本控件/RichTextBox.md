# RichTextBox

```
<RichTextBox Name="rr"  VerticalScrollBarVisibility="Auto" HorizontalAlignment="Left" Height="474" Margin="128,299,0,0" VerticalAlignment="Top" Width="543" Grid.ColumnSpan="2"/>

rr.Document.Blocks.Add(new Paragraph(new Run("bbbb") { Foreground = Brushes.Green }));
rr.ScrollToEnd();

rr.Document.Blocks.Clear();
```
