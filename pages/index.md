---
sales_target: 300
---



```sql sales_by_country
select 'Canada' as country, 100 as sales
union all
select 'US' as country, 250 as sales
union all
select 'UK' as country, 130 as sales
union all
select 'Australia' as country, 95 as sales
```

```sql test_data
select country as name, sales as value
from ${sales_by_country}
```

<ECharts config={
    {
      title: {
        text: 'Treemap Example',
        left: 'center'
      },
        tooltip: {
            formatter: '{b}: {c}'
        },
      series: [
        {
          type: 'treemap',
          visibleMin: 300,
          label: {
            show: true,
            formatter: '{b}'
          },
          itemStyle: {
            borderColor: '#fff'
          },
          roam: false,
          nodeClick: false,
          data: data.test_data,
          breadcrumb: {
            show: false
          }
        }
      ]
      }
    }
    on:click={click_handler}
/>

{$name}

```sql filtered_sales
select *
from ${sales_by_country}
where country = '${$name}'
```

<DataTable data={filtered_sales} />

```sql filtered_sales2
select '${sales_target}' as name
```



<script>

    // Create a writable store for name
    let name = writable('');

    let other_name = 2;

    function click_handler(ev) {
        if (ev.detail && ev.detail.data) {
            // Update the store with the new name
            name.set(ev.detail.data.name);
            console.log($name); // Log the current value of the name
        }
    }
</script>
