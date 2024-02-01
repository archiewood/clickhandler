---
growth: 0.05
target: 400
---

# Will we make our sales target?

<Dropdown name=growth_rate title="Growth Rate">
    <DropdownOption value="0.05" valueLabel="5%"/>
    <DropdownOption value="0.1" valueLabel="10%"/>
    <DropdownOption value="0.15" valueLabel="15%"/>
    <DropdownOption value="0.2" valueLabel="20%"/>
</Dropdown>

<Dropdown name=start_value title="Start Value">
    <DropdownOption value="100" valueLabel="100"/>
    <DropdownOption value="200" valueLabel="200"/>
    <DropdownOption value="300" valueLabel="300"/>
    <DropdownOption value="400" valueLabel="400"/>
</Dropdown>



```sql growth_in_sales
select ${inputs.start_value} as sales, 2001 as year
union all
select ${inputs.start_value} * (1+${inputs.growth_rate}) as sales, 2002 as year
union all
select ${inputs.start_value} * (1+ ${inputs.growth_rate})^2 as sales, 2003 as year
union all
select ${inputs.start_value} * (1+ ${inputs.growth_rate})^3 as sales, 2004 as year
union all
select ${inputs.start_value} * (1+ ${inputs.growth_rate})^4 as sales, 2005 as year
union all
select ${inputs.start_value} * (1+ ${inputs.growth_rate})^5 as sales, 2006 as year
union all
select ${inputs.start_value} * (1+ ${inputs.growth_rate})^6 as sales, 2007 as year
union all
select ${inputs.start_value} * (1+ ${inputs.growth_rate})^7 as sales, 2008 as year
union all
select ${inputs.start_value} * (1+ ${inputs.growth_rate})^8 as sales, 2009 as year
order by year
```

<LineChart title="Projected Revenue" data={growth_in_sales} x="year" y="sales" yMax={growth_in_sales[data.growth_in_sales.length - 1].sales < 500 ? 500 : growth_in_sales[data.growth_in_sales.length - 1].sales} yFmt='"$"0"M"'>
<ReferenceLine y={target} label=Target />
</LineChart>

{#if growth_in_sales[data.growth_in_sales.length - 1].sales > 2*target}

<Alert status=success>
  To the moon! 🚀🚀🚀
</Alert>


{:else if growth_in_sales[data.growth_in_sales.length - 1].sales > target}

<Alert status=success>
  Success! The sales have exceeded the target.
</Alert>

{:else}

<Alert status=warning>
  Not quite there yet. Keep pushing!
</Alert>

{/if}




