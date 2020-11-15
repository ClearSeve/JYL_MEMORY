# DatePicker

```
<DatePicker x:Name="datePickerCtl" Width="200" Height="25"
            SelectedDateFormat="Long" FirstDayOfWeek="Monday"
            DisplayDateStart="2020/1/1" DisplayDateEnd="2020/1/20"
            IsTodayHighlighted="False">
    <DatePicker.BlackoutDates>
        <CalendarDateRange Start="2020/1/3" End="2020/2/5"/>
        <CalendarDateRange Start="2020/1/10" End="2020/2/6"/>
    </DatePicker.BlackoutDates>
</DatePicker>
```