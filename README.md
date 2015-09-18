<img src="/images/ic_launcher-web.png" width="300px" />

Material Calendar View [![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-Material%20Calendar%20View-blue.svg?style=flat)](https://android-arsenal.com/details/1/1531)
======================

A Material design back port of Android's CalendarView. The goal is to have a Material look
and feel, rather than 100% parity with the platform's implementation.

<img src="/images/screencast.gif" alt="Demo Screen Capture" width="300px" />

Major Change in 1.0.0
---------------------

With the implementation of multiple selection, some of the apis needed to change to support it,
namely `OnDateChangedListener` is now `OnDateSelectedListener`. There are also a bunch of new apis
for multiple selection.

Also, `showOtherDates` is now a set of flags for finer control over which extra dates are shown.

Major Change in 0.8.0
---------------------

The view now responds better to layout parameters.
The functionality is similar to how `adjustViewBounds` works with ImageView,
where the view will try and take up as much space as necessary,
but we base it on tile size instead of an aspect ratio.
The exception being that if a `tileSize` is set,
that will override everything and set the view to that size.

Usage
-----

1. Add `compile 'com.prolificinteractive:material-calendarview:1.0.0'` to your dependencies.
2. Add `MaterialCalendarView` into your layouts or view hierarchy.
3. Set a `OnDateSelectedListener` or call `MaterialCalendarView.getSelectedDates()` when you need it.

[Javadoc Available Here](http://prolificinteractive.github.io/material-calendarview/)

Example:

```xml
<com.prolificinteractive.materialcalendarview.MaterialCalendarView
    android:id="@+id/calendarView"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    app:mcv_showOtherDates="boolean"
    app:mcv_arrowColor="color"
    app:mcv_selectionColor="color"
    app:mcv_headerTextAppearance="style"
    app:mcv_dateTextAppearance="style"
    app:mcv_weekDayTextAppearance="style"
    app:mcv_weekDayLabels="array"
    app:mcv_monthLabels="array"
    app:mcv_tileSize="dimension"
    app:mcv_firstDayOfWeek="enum"
    app:mcv_leftArrowMask="drawable"
    app:mcv_rightArrowMask="drawable"
    />
```

Customization
-------------

One of the aims of this library is to be customizable.

Options available in Java and as XML attributes:

| Attribute             | Type      | Description                                                                                                                                                                                                     |
|:----------------------|:----------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| showOtherDates        | flags     | By default, only days of one month, in the min-max range, are shown. There are flags for `other_months`, `out_of_range`, and `decorated_disabled`. There are also `none`, `all`, or `defaults` for convenience. |
| arrowColor            | color     | Set the color of the arrows used to page the calendar. Black by default.                                                                                                                                        |
| selectionColor        | color     | Set the color of the date selector. By default this is the color set by`?android:attr/colorAccent` on 5.0+ or `?attr/colorAccent` from the AppCompat library.                                                   |
| headerTextAppearance  | style     | Override the text appearance of the month-year indicator at the top.                                                                                                                                            |
| weekDayTextAppearance | style     | Override the text appearance of the week day indicators.                                                                                                                                                        |
| dateTextAppearance    | style     | Override the text appearance of the dates.                                                                                                                                                                      |
| weekDayLabels         | array     | Supply custom labels for the days of the week. This sets an `ArrayWeekDayFormatter` on the `CalendarView`.The default uses Java's `Calendar` class to get a `SHORT` display name.                               |
| monthLabels           | array     | Supply custom labels for the months of the year. This sets a `MonthArrayTitleFormatter` on the `CalendarView`.The default implementation formats using `SimpleDateFormat` with a `"MMMM yyyy"` format.          |
| tileSize              | dimension | Set a custom size for each tile. Each day of the calendar is 1 tile, and the top bar is 1 tile high.The entire widget is 7 tiles by 8 tiles. The default tile size is `44dp`.                                   |
| firstDayOfWeek        | enum      | Set the first day of the month                                                                                                                                                                                  |
| leftArrowMask         | drawable  | Supply a different drawable mask for the left arrow                                                                                                                                                             |
| rightArrowMask        | drawable  | Supply a different drawable mask for the right arrow                                                                                                                                                            |

Options only available in Java:

| Method             | Description                                                                 |
|:-------------------|:----------------------------------------------------------------------------|
| setMinimumDate()   | Set the earliest visible date on the calendar                               |
| setMaximumDate()   | Set the latest visible date on the calendar                                 |
| setSelectedDate()  | Set the date to show as selected. Must be within minimum and maximum dates. |
| setTopbarVisible() | Set the top bar (arrows and title) as visible or gone.                      |
| setSelectionMode() | Set the mode of selection. Choices are single, multiple or none.            |

### Date Selection

We support three modes of selection: single, multiple, or none. The default is single selection.
The mode can be changed by calling `setSelectionMode()` and passing the approprate constant (`SELECTION_MODE_NONE`, `SELECTION_MODE_SINGLE`, or `SELECTION_MODE_MULTIPLE`).
If you change to single selection, all selected days except the last selected will be cleared.
If you change to none, all selected days will be cleared.

You can set an `OnDateSelectedListener` to listen for selections, make sure to take into account multiple calls for the same date and state.
You can manually select or deselect dates by calling `setDateSelected()`.
There is also `setSelectedDate()` will will clear the current selection(s) and select the provided date.

There are also: `clearSelection()`, `getSelectedDates()`, and `getSelectionMode()`; which should be self-explanitory.


### Events, Highlighting, Custom Selectors, and More!

Material CalendarView provides an API that allows you to modify the appearance of a set of days.
The `DayViewDecorator` API allows you to:

* Set a custom background drawable
* Set a custom selector drawable
* Apply spans the the entire day's text
    * See [this tutorial](http://blog.stylingandroid.com/introduction-to-spans/) for spans on Android
    * We provide `DotSpan` which will draw a dot centered below the text
* Set dates as disabled

To do so, you need to make a new instance of `DayViewDecorator` and add it to the calendar with `addDecorator()`.
Decorating is done via a `DayViewFacade` that is passed to the `decorate()` method.
All calls to the `DayViewFacade` will be applied to every day where `shouldDecorate()` returns true.

`DayViewFacade` has four methods to allow decoration:

1. `setBackgroundDrawable()` set a drawable to draw behind everything else. This also responds to state changes.
2. `setSelectionDrawable()` allows customizes the selection indicator for specific days.
3. `addSpan()` sets a span on the entire day label.
    * `DotSpan` was added to show a dot centered below the label
    * For an introduction to spans, see [this article](http://androidcocktail.blogspot.com/2014/03/android-spannablestring-example.html).
    * If you want to learn more about custom spans, check out [this article](http://flavienlaurent.com/blog/2014/01/31/spans/).
4. `setDaysDisabled()` allows you to disable and re-enable days. This will not affect minimum and maximum dates.

If one of your decorators changes after it's been added to the calendar view, make sure you call `MaterialCalendarView.invalidateDecorators()` to have those changes reflected.

When implementing a `DayViewDecorator`, make sure that they are as efficent as possible.
Remember that `shouldDecorate()` needs to be called 42 times for each month view.
An easy way to be more efficent is to convert your data to `CalendarDay`s outside of `shouldDecorate()`.

Check out the sample app's `BasicActivityDecorated` activity for some examples.

Contributing
============

Would you like to contribute? Fork us and send a pull request! Be sure to checkout our issues first.

License
=======

>Copyright 2015 Prolific Interactive
>
>Licensed under the Apache License, Version 2.0 (the "License");
>you may not use this file except in compliance with the License.
>You may obtain a copy of the License at
>
>   http://www.apache.org/licenses/LICENSE-2.0
>
>Unless required by applicable law or agreed to in writing, software
>distributed under the License is distributed on an "AS IS" BASIS,
>WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
>See the License for the specific language governing permissions and
>limitations under the License.
