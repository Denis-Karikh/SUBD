```sql
Select sum(points),year_game from statistic
group by year_game
```

```sql
with stat as (Select sum(points),year_game from statistic
group by year_game)
Select * from stat
```

```sql
with stat as (Select sum(points) as point_current_year,year_game,lag(sum(points))over (order by year_game) as prev_year from statistic
group by year_game)
Select * from statistic
inner join stat on stat.year_game = statistic.year_game
```
