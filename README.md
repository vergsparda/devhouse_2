# devhouse_2
## **Task**
### Write a separe script that calls: [link](http://api.dev.cakeiteasy.no/api/store/bakeries/test-bakery-pay-in-store/?country=NO) and transforms schedule in the response
----------------------------------------
### ![schedule](src\assets\scheduele.PNG)
---------------------------------------
### Into array of strings in the format:
---------------------------------------
```
__For {dayNameStart} - {dayNameEnd}: before {order_before}, {days_before_order}
day(s) before__
```

```
For monday-friday: Before 13:00, 1 day before
```

#### So the main challenge: group similar days: don’t show schedule for each day if a
previous day has the same rules. So in the screenshot above Monday-Friday is the
same, Saturday and Sunday is different
----
#### For day offs please use format

```
{dayName}: closed

```
## Test with other bakeries from the list available via [bakeries](http://api.dev.cakeiteasy.no/api/store/bakeries/?country_code=no)
Example of possible UI

```
For monday: before 12:00, 2 day(s) before
For tuesday: before 13:00, 1 day(s) before
For wednesday: before 14:00, 2 day(s) before
For thursday - friday: before 12:00, 1 day(s) before
For saturday: before 12:00, 3 day(s) before
For sunday: before 12:00, 1 day(s) before

```
```
## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```
