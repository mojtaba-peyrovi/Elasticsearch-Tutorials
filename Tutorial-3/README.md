### Exploring data sets with Kibana by Nicolas Fränkel:

This is the [link](https://www.youtube.com/watch?v=73llapmOhlM) to the YouTube video.

---

When we open data in Discover, we can easily have a quick search in the search bar for example we can say:
```
    field_name: <criteria>  e.g. day_of_week : "Thursday"
```
And it shows the documents having this keyword.

Here is another example of multiple condition:
```
customer_gender: FEMALE AND NOT(day_of_week:Wednesday)
```
And one more:
````
customer_gender: FEMALE AND(day_of_week:Wednesday) AND NOT(category:"Women's Clothing")
````

If we need to define range we do it like this:
```
customer_gender: FEMALE AND total_quantity:[* TO 4] // shows all smaller than 4
customer_gender: FEMALE AND total_quantity:[2 TO *]  // shows all bigger than 2
customer_gender: FEMALE AND total_quantity:[2 TO 4] // shows all between 2, 4
```
We can save our results and use them in dashboard. In Dashboard we will push __ADD__ and under it, there is __SAVED SEARCH__ so that we can import our filters result.

__Timelion visualuzation:__ It shows the timeseries for different queries. like this:
```
.es(q=customer_gender:FEMALE, index=kibana_sample_data_ecommerce, timefield=order_date)
```
The timefield should be mentioned in the field name, otherwise we need to explicitly mention it here.

We can aggregate chart data like this:
```
.es(q=products.manufacturer:Microlutions, index=kibana_sample_data_ecommerce, timefield=order_date).multiply(2)
```
It multiplies all values in the chart to 2.

