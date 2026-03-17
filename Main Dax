# Dax-Date-Table-
Dax Measures
DateTable =
VAR MinDate = DATE(2015, 1, 1)
VAR MaxDate = DATE(2030, 12, 31)
RETURN
ADDCOLUMNS(
    CALENDAR(MinDate, MaxDate),
    "Year", YEAR([Date]),
    "Month Number", MONTH([Date]),
    "Month Name", FORMAT([Date], "MMMM"),
    "Month Short", FORMAT([Date], "MMM"),
    "Quarter", "Q" & FORMAT([Date], "Q"),
    "Year-Month", FORMAT([Date], "YYYY-MM"),
    "Week Number", WEEKNUM([Date], 2),
    "Day", DAY([Date]),
    "Day Name", FORMAT([Date], "DDDD"),
    "Day Short", FORMAT([Date], "DDD"),
    "Is Weekend", IF(WEEKDAY([Date], 2) > 5, TRUE(), FALSE())
)
