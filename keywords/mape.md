# Mape

Parents: [[loss]]
Relevant:

#loss


**Mean Average Percentage Error**, a loss that is beloved in business; for example in product supply and sales forecasts, when values are positive only.

`(Actual - Forecast)/Actual` - and then convert to percent, and do the avearge of all of it. Works well only at aggregate data, of course, as you can't divide on zero, and individual forecasts for individual products are often zeros. But if you do it by broader unit of time, and per category, it kinda makes sense.