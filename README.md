# CRIME-ANALYSIS

SELECT *, sum(Rape) over(partition by Year) as total_rape_year 
FROM pop.crimes
where state = 'gujarat';


With Final as(
select  upper(State) as State_Name, Year, Rape,   DD,AoW,AoM, DV,WT 
from pop.crimes)
select *
from Final;

With cte as(
SELECT *, sum(Rape) over(partition by State,year) as total_rape_year 
FROM pop.crimes)
select *
from cte
order by total_rape_year desc;

With cte as(
select *, Rape+DD+AoW+AoM+DV+WT as Total_Cases
from pop.crimes), 2001_ as(
select  Year,State, Sum(Total_Cases) as Total_Case
from cte
where year in('2001')
group by Year,State), 2021_ as(
select  Year,State, Sum(Total_Cases) as Total_Case
from cte
where year in('2021')
group by Year,State), Dimention as(
select  f1.Year as year_2021, f1.State, f1.total_case as total_case_2021, f2.year as year_2001, f2.total_case as total_case_2001
from 2021_ f1
join 2001_ f2
on f1.State = f2.state), High as(
select  *,  round((total_case_2021-total_case_2001)/total_case_2001*100,1) as diff
from Dimention)
select *
from High
order by diff desc;


select *, sum(rape) over(order  by year) as cum_sum 
from pop.crimes
where state in('Gujarat');


with cte1 as(
select *
from pop.crimes
where Rape < 50), cte2 as(
select *
from pop.crimes
where Rape > 5000)
select  f1.state, f1.year, f1.rape,  f2.year as Year_2, f2.rape as Rape_2
from cte1 f1
join cte2 f2
on f1.state = f2.state;
 
 
select *
from pop.crimes;
