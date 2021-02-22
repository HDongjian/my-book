# 日期设置常用方法

```js
/**
 * 日期格式化方法
 * @param {日期} date
 * @param {string} formatStr 日期格式
 */
dateFormate (date = new Date(), formatStr = 'YYYY-MM-DD HH:mm:ss') {
  date = new Date(date)
  if (!this.isString(formatStr)) {
    formatStr = 'YYYY-MM-DD HH:mm:ss'
  }
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
      case 'mouth':
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
},
/**
 *获取日期的前一天
  * @param {日期} date
  */
dateYesterday (date = new Date()) {
  date = new Date(date)
  var mouth = Number(date.getMonth()) - 1
  return new Date(date.getFullYear() + '-' + mouth + '-' + date.getDate() + ' 00:00:00')
},
/**
 *获取日期的后一天
  * @param {日期} date
  */
dateTomorrow (date = new Date()) {
  date = new Date(date)
  date.setDate(date.getDate() + 1)
  var mouth = Number(date.getMonth()) + 1
  return new Date(date.getFullYear() + '-' + mouth + '-' + date.getDate() + ' 00:00:00')
},
/**
 *获取日期所在周的第一天
  * @param {日期} date
  */
dateWeekFirstDay (date = new Date()) {
  date = new Date(date)
  var WeekFirstDay = new Date(date - (date.getDay() - 1) * 86400000)
  var mouth = Number(WeekFirstDay.getMonth()) + 1
  return new Date(WeekFirstDay.getFullYear() + '-' + mouth + '-' + WeekFirstDay.getDate() + ' 00:00:00')
},
/**
 *获取日期所在周的最后一天
  * @param {日期} date
  */
dateWeekLastDay (date = new Date()) {
  date = new Date(date)
  var WeekFirstDay = new Date(date - (date.getDay() - 1) * 86400000)
  var WeekLastDay = new Date((WeekFirstDay / 1000 + 6 * 86400) * 1000)
  var mouth = Number(WeekLastDay.getMonth()) + 1
  return new Date(date.getFullYear() + '-' + mouth + '-' + WeekLastDay.getDate() + ' 23:59:59')
},
/**
 *获取日期所在月的第一天
  * @param {日期} date
  */
dateMonthFirstDay (date = new Date()) {
  date = new Date(date)
  var MonthFirstDay = new Date(date.getFullYear(), date.getMonth(), 1)
  var mouth = Number(MonthFirstDay.getMonth()) + 1
  return new Date(MonthFirstDay.getFullYear() + '-' + mouth + '-' + MonthFirstDay.getDate() + ' 00:00:00')
},
/**
 *获取日期所在月的最后一天
  * @param {日期} date
  */
dateMonthLastDay (date = new Date()) {
  date = new Date(date)
  var MonthNextFirstDay = new Date(date.getFullYear(), date.getMonth() + 1, 1)
  var MonthLastDay = new Date(MonthNextFirstDay - 86400000)
  var mouth = Number(MonthLastDay.getMonth()) + 1
  return new Date(MonthLastDay.getFullYear() + '-' + mouth + '-' + MonthLastDay.getDate() + ' 23:59:59')
},
/**
 * 根据时间格式获取两个时间差
 * @param {开始时间} startTime
 * @param {结束时间} endTime
 * @param {格式要求} formatStr
 */
dateDifference (startTime = new Date(), endTime = new Date(), formatStr = 'Y天H小时M分S秒') {
  startTime = new Date(startTime); endTime = new Date(endTime); formatStr = formatStr.toUpperCase()
  var leave1, leave2, leave3
  var difference = endTime.getTime() - new Date(startTime).getTime() // 时间差的毫秒数
  if (difference < 0) {
    throw new Error('The start time cannot be greater than the end time(开始时间不能大于结束时间)')
  }
  if (formatStr.indexOf('D') >= 0) {
    var days = Math.floor(difference / (24 * 3600 * 1000))
    leave1 = difference % (24 * 3600 * 1000) // 计算天数后剩余的毫秒数
  } else {
    leave1 = difference
  }
  if (formatStr.indexOf('H') >= 0) {
    var hours = Math.floor(leave1 / (3600 * 1000))
    leave2 = leave1 % (3600 * 1000) // 计算小时数后剩余的毫秒数
  } else {
    leave2 = leave1
  }
  if (formatStr.indexOf('M') >= 0) {
    var minutes = Math.floor(leave2 / (60 * 1000))
    leave3 = leave2 % (60 * 1000) // 计算分钟数后剩余的毫秒数
  } else {
    leave3 = leave2
  }
  if (formatStr.indexOf('S') >= 0) {
    var seconds = Math.round(leave3 / 1000)
  }
  return formatStr.replace(/D{1,2}|d{1,4}|H{1,2}|m{1,2}|M{1,2}|s{1,2}|S{1,2}/g, (match) => {
    switch (match) {
      case 'D':
        return String(days)
      case 'DD':
        return String(days).padStart(2, '0')
      case 'd':
        return String(days)
      case 'dd':
        return String(days).padStart(2, '0')
      case 'H':
        return String(hours)
      case 'HH':
        return String(hours).padStart(2, '0')
      case 'h':
        return String(hours)
      case 'hh':
        return String(hours).padStart(2, '0')
      case 'M':
        return String(minutes)
      case 'MM':
        return String(minutes).padStart(2, '0')
      case 'm':
        return String(minutes)
      case 'mm':
        return String(minutes).padStart(2, '0')
      case 'S':
        return String(seconds)
      case 'SS':
        return String(seconds).padStart(2, '0')
      case 's':
        return String(seconds)
      case 'ss':
        return String(seconds).padStart(2, '0')
      default:
        return match
    }
  })
}
```
