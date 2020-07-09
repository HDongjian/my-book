## 日期格式化

```js
myMoment (date = new Date().getTime()) {
    this.date = new Date(date)
    return this
  },
  formate (formatStr = 'YYYY-MM-DD HH:mm:ss') {
    const date = this.date
    const year = date.getFullYear()
    const month = date.getMonth() + 1
    const day = date.getDate()
    const week = date.getDay()
    const hour = date.getHours()
    const minute = date.getMinutes()
    const second = date.getSeconds()
    const weeks = ['日', '一', '二', '三', '四', '五', '六']
    return formatStr.replace(/Y{2,4}|M{1,2}|D{1,2}|d{1,4}|H{1,2}|m{1,2}|s{1,2}/g, (match) => {
      switch (match) {
        case 'YY':
          return String(year).slice(-2)
        case 'YYY':
        case 'YYYY':
          return String(year)
        case 'M':
          return String(month)
        case 'MM':
          return String(month).padStart(2, '0')
        case 'D':
          return String(day)
        case 'DD':
          return String(day).padStart(2, '0')
        case 'd':
          return String(week)
        case 'dd':
          return weeks[week]
        case 'ddd':
          return '周' + weeks[week]
        case 'dddd':
          return '星期' + weeks[week]
        case 'H':
          return String(hour)
        case 'HH':
          return String(hour).padStart(2, '0')
        case 'm':
          return String(minute)
        case 'mm':
          return String(minute).padStart(2, '0')
        case 's':
          return String(second)
        case 'ss':
          return String(second).padStart(2, '0')
        default:
          return match
      }
    })
  }
```

## 获取当前日期的周边

### 今天
```js
function showToDay() {
    var Nowdate = new Date();
    M = Number(Nowdate.getMonth()) + 1
    return Nowdate.getFullYear() + "-" + M + "-" + Nowdate.getDate();
}
```
### 明天

```js
function showTomorrow() {
    var tom = new Date();
    tom.setDate(tom.getDate() + 1);
    M = Number(tom.getMonth()) + 1
    return tom.getFullYear() + "-" + M + "-" + tom.getDate();
}
```
### 本周第一天
```js
function showWeekFirstDay() {
    var Nowdate = new Date();
    var WeekFirstDay = new Date(Nowdate - (Nowdate.getDay() - 1) * 86400000);
    M = Number(WeekFirstDay.getMonth()) + 1
    return WeekFirstDay.getFullYear() + "-" + M + "-" + WeekFirstDay.getDate();
}
```
### 本周最后一天
```js
function showWeekLastDay() {
    var Nowdate = new Date();
    var WeekFirstDay = new Date(Nowdate - (Nowdate.getDay() - 1) * 86400000);
    var WeekLastDay = new Date((WeekFirstDay / 1000 + 6 * 86400) * 1000);
    M = Number(WeekLastDay.getMonth()) + 1
    return Nowdate.getFullYear() + "-" + M + "-" + WeekLastDay.getDate();
}
```
### 本月第一天
```js
function showMonthFirstDay() {
    var Nowdate = new Date();
    var MonthFirstDay = new Date(Nowdate.getFullYear(), Nowdate.getMonth(), 1);
    M = Number(MonthFirstDay.getMonth()) + 1
    return MonthFirstDay.getFullYear() + "-" + M + "-" + MonthFirstDay.getDate();
}
```
### 本月最后一天
```js
function showMonthLastDay() {
    var Nowdate = new Date();
    var MonthNextFirstDay = new Date(Nowdate.getFullYear(), Nowdate.getMonth() + 1, 1);
    var MonthLastDay = new Date(MonthNextFirstDay - 86400000);
    M = Number(MonthLastDay.getMonth()) + 1
    return MonthLastDay.getFullYear() + "-" + M + "-" + MonthLastDay.getDate();
}
```
