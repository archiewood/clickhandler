---
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
with recursive yearly_sales as (
    select ${inputs.start_value} as sales, 2001 as year
    union all
    select sales * (1 + ${inputs.growth_rate}), year + 1
    from yearly_sales
    where year < 2009
)
select * from yearly_sales
```

<LineChart title="Projected Revenue" data={growth_in_sales} x="year" y="sales" yMax={growth_in_sales[8].sales < 500 ? 500 : growth_in_sales[8].sales} yFmt='"$"0"M"'>
  <ReferenceLine y={target} label=Target />
</LineChart>

{#if growth_in_sales[8].sales > 2*target}
  <Alert status=success>
    To the moon! 🚀🚀🚀
  </Alert>
{:else if growth_in_sales[8].sales > target}
  <Alert status=success>
    Success! The sales have exceeded the target.
  </Alert>
{:else}
  <Alert status=warning>
    Not quite there yet. Keep pushing!
  </Alert>
{/if}