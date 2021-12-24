# sql_code
--Previewing Dataset

select *
from First_Portfolio.dbo.covid_death ;


--Selecting columns for Exploration

select location,date,total_cases,total_deaths,new_cases,population
from First_Portfolio.dbo.covid_death 
order by location,date;


--looking at Death rate per Cases 

select location,date,(total_deaths/total_cases)*100 as death_case_percent,population
from First_Portfolio.dbo.covid_death 
order by location,date;

-- looking at Death rate per Population

select location,date,(total_deaths/population)*100 as death_population_percent,population
from First_Portfolio.dbo.covid_death 
order by location,date;

--checking correlation between deaths/cases and deaths/population

select location,
		date,
		(total_deaths/total_cases)*100 as death_case_percent,
		total_cases,
		(total_deaths/population)*100 as death_population_percent,
		population
from First_Portfolio.dbo.covid_death 
order by location,date;


--looking at Total cases and deaths

select  location, sum(new_cases)as total_cases, sum(new_deaths) as total_death
from First_Portfolio.dbo.covid_death 
where  continent is not null and location is not null and new_cases is not null and new_deaths is not null-- and date is not null  and date between '2020-01-01' and '2020-12-31'
group by location
order by location


-- Previewing vaccination table
select *
from First_portfolio.dbo.covid_vaccine;


-- looking at Fully vaccinated people

select sum(cast(people_fully_vaccinated as float)) as vaccinated_people, location
from First_portfolio.dbo.covid_vaccine
where date between '2021-01-01' and '2021-12-30'
group by location
order by location;


-- joining Two tables (Covid Death and Covid vaccine) for India
-- joined using location and date as keys


select death.continent,death.location, death.new_cases, death.new_deaths , cast( vaccine.people_fully_vaccinated as float) as vaccinated_people
from First_Portfolio.dbo.covid_death as death
 inner join  First_portfolio.dbo.covid_vaccine as vaccine
     on death.location = vaccine.location and death.date = vaccine.date
	where death.continent is not null and death.new_cases is not null and death.new_deaths is not null and people_fully_vaccinated is not null
	--group by death.continent,death.location,vaccine.people_fully_vaccinated
order by death.continent, death.location



	   
