Designed to be used as a Vue component: `Vue.component(element, Calus)` where element is the name of the template i.e. 'calendar-calus'. You can then use:
```html
<calendar-calus inline-template></calendar-calus>
```

### Props
You can pass the following props from the template:
- `time-zone`: defaults to local otherwise can be explicitly set (i.e. Europe/London)
- `available-dates`: list of dates which are available to select (luxon DateTime format)
- `display-in-column`: whether to show all the months in a column, or a single month with controls to change which month is shown
- `linear-dates`: linear view for when showing a free flowing calendar
- `week-starts-on-sunday`: force the day to start on sunday instead of default monday. Accepts true/false
- `on-select`: callback for when an available date is clicked
- `allow-scrolling-outside-of-date-range`: defaults to false, set to true if you want to be able to scroll to months outside of available date range
- `month-controls-classes`: array of classes that control the previous/next month buttons i.e. `['.month__control--prev', '.month__control--next']`. This is used in conjunction with `display-in-column: false` and `allow-scrolling-outside-of-date-range: false` and sets a `disabled` class on the appropriate previous/next button

### Methods
- `select()`: method to call when clicking on a date. Uses function passed into prop `on-select` for logic
- `scrollMonth()`: pass any number (positive or negative) to scrolle forward/previous n months

### Example Template
```html
<calendar-calus
    inline-template
    :allow-scrolling-outside-of-date-range="false"
	time-zone="America/Chicago"
	:month-controls-classes="['.month__control--prev', '.month__control--next']"
>
    <div class="month-container" v-bind:class="{ 'month-container--column': displayInColumn }">
        <div class="month" v-for="month in months" v-bind:data-month="month.time.toFormat('MM/y')">
            <div class="month__header">
            <button type="button" class="month__control month__control--prev" :disabled="month.disablePrevScroll" v-if="!displayInColumn" v-on:click="scrollMonth(-1)">
                ‹
            </button>
            <div class="month__text">
                {{ month.time.toFormat(month.isInCurrentYear ? 'MMMM' : 'MMMM y') }}
            </div>
            <button type="button" class="month__control month__control--next" v-if="!displayInColumn" v-on:click="scrollMonth(1)">
                ›
            </button>
            </div>
            <div class="month__days">
            <div class="day"
                v-for="day in month.days"
                v-bind:data-date="+day.time"
                v-bind:class="{
                'is-today': day.isToday,
                'is-not-today': !day.isToday,
                'is-available': day.isAvailable,
                'is-not-available': !day.isAvailable,
                'is-past': day.isPast,
                'is-future': day.isFuture,
                'is-selected': day.isSelected,
                'is-this-month': day.isThisMonth,
                'is-different-month': !day.isThisMonth,
                }"
                v-on:click="select(day)"
            >
                <div class="day__inner">
                    {{ day.time.day }}
                </div>
            </div>
            </div>
        </div>
    </div>
</calendar-calus>
```
