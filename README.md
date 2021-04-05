# How to select a week in the Flutter data range picker (SfDateRangePicker)

In the flutter date range picker, you can select the entire week when selecting a single day using the `onSelectionChanged` callback and `enableSwipeSelection` property.

## Step 1:
Inside the state, initialize the controller for date range picker.

```xml
final DateRangePickerController _controller = DateRangePickerController();
```
## Step 2:
Place the date range picker inside the body of the Scaffold widget, set the `selectionMode` as range selection and trigger the `onSelectionChanged` callback of the flutter date picker.
`
```xml
return Scaffold(
  body: Column(
    children: <Widget>[
      Card(
        margin: const EdgeInsets.fromLTRB(50, 100, 50, 100),
        child: SfDateRangePicker(
          controller: _controller,
          view: DateRangePickerView.month,
          selectionMode: DateRangePickerSelectionMode.range,
          onSelectionChanged: selectionChanged,
          monthViewSettings: DateRangePickerMonthViewSettings(enableSwipeSelection: false),
        ),
      )
    ],
  ), 
);
```

## Step 3:
Find the `startDate` and `endDate` from the `onSelectionChanged` event args. Calculate the first day of the week for `startDate`, `endDate`, and add the remaining dates to the start and end dates for the week selection. And assign those dates to the `selectedRange` property of controller. Please find the code for the same.

```xml
void selectionChanged(DateRangePickerSelectionChangedArgs args) {
  int firstDayOfWeek = DateTime.sunday % 7;
  int endDayOfWeek = (firstDayOfWeek - 1) % 7;
  endDayOfWeek = endDayOfWeek < 0? 7 + endDayOfWeek : endDayOfWeek;
  PickerDateRange ranges = args.value;
  DateTime date1 = ranges.startDate!;
  DateTime date2 = (ranges.endDate?? ranges.startDate)!;
  if(date1.isAfter(date2))
    {
      var date=date1;
      date1=date2;
      date2=date;
    }
  int day1 = date1.weekday % 7;
  int day2 = date2.weekday % 7;
 
  DateTime dat1 = date1.add(Duration(days: (firstDayOfWeek - day1)));
  DateTime dat2 = date2.add(Duration(days: (endDayOfWeek - day2)));
 
  if( !isSameDate(dat1, ranges.startDate)|| !isSameDate(dat2,ranges.endDate))
  {
    _controller.selectedRange = PickerDateRange(dat1, dat2);
  }
}
```
**[View document in Syncfusion Flutter Knowledge base](https://www.syncfusion.com/kb/11412/how-to-select-a-week-in-the-flutter-data-range-picker-sfdaterangepicker)**

**Screenshots**

<img alt="week selection"  src="http://www.syncfusion.com/uploads/user/kb/flut/flut-861/flut-861_img1.png" width="230" height="400" />|
