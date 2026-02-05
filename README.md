# Credit-Risk-and-Loan-Approval-Analysis
Address asymmetric gaps between lenders and borrowers to predict probability of default

# Data Analysis on each group of loan intent
select loan_intent, 
count(*) as total_loan,
sum(loan_status) as Loan_Default,
avg(loan_status) as Percent_of_Default,
round(stddev(loan_status), 3) as Standard_Deviation
from credit_risk_dataset
group by loan_intent
order by Percent_of_Default desc;

# Data Analysis for each age 
select person_age, 
count(*) as total_loan,
sum(loan_status) as Loan_Default,
avg(loan_status) as Percent_of_Default,
round(stddev(loan_status), 3) as Standard_Deviation
from credit_risk_dataset
group by person_age
order by Percent_of_Default desc;

# Average interest rate by credit grade
select avg(loan_int_rate) as Average_Interest_Rate,
loan_grade,
avg(loan_status) as Default_Percentage
from credit_risk_dataset
group by loan_grade
order by loan_grade;

# Average interest rate by home ownership status 
select loan_grade,
count(*) as Total_loan,
avg(loan_int_rate) as Average_Interest_Rate,
avg(loan_status) as Default_Percentage,
stddev(loan_status) as Standard_Deviation
from credit_risk_dataset
group by loan_grade
order by loan_grade;

# Credit history and loan default percentage
select cb_person_cred_hist_length,
count(*) as Total_loan,
avg(loan_status) as Default_Percentage,
stddev(loan_status)
from credit_risk_dataset
group by cb_person_cred_hist_length
order by Default_Percentage desc;

# Loan default percentage for loan to income ratio 
select 
case
when (loan_amnt / person_income) < .1 then "0% to 10%"
when (loan_amnt / person_income) < .2 then "10% to 20%"
when (loan_amnt / person_income) < .3 then "20% to 30%"
when (loan_amnt / person_income) < .4 then "30% to 40%"
when (loan_amnt / person_income) < .5 then "40% to 50%"
else "+50%"
end as loan_to_income_ratio,
count(*) as total_loan,
avg(loan_status) as Default_Percentage,
stddev(loan_status)
from credit_risk_dataset
group by loan_to_income_ratio
order by Default_Percentage desc;

# Loan default percentage for loan to income ratio 2
select 
case
when (loan_amnt / person_income) < .2 then "<20%"
when (loan_amnt / person_income) between .2 and .25 then "20% to 25%"
when (loan_amnt / person_income) between .25 and .3 then "25% to 30%"
when (loan_amnt / person_income) between .3 and .35 then "30% to 35%"
when (loan_amnt / person_income) between .35 and .4 then "35% to 40%"
else "+40%"
end as loan_to_income_ratio,
count(*) as total_loan,
avg(loan_status) as Default_Percentage,
stddev(loan_status)
from credit_risk_dataset
group by loan_to_income_ratio
order by Default_Percentage desc;

#Risk Category
WITH borrower_metrics AS (
    SELECT *,
        loan_amnt / person_income AS income_to_loan_ratio
    FROM credit_risk_dataset
)
SELECT
    CASE
        when income_to_loan_ratio > 0.3 THEN 'High Risk'
        when income_to_loan_ratio > 0.25 THEN 'Moderate Risk'
        ELSE 'Low Risk'
    END AS risk_category,
    COUNT(*) AS total_loans,
    AVG(loan_status) AS default_rate
FROM borrower_metrics
GROUP BY risk_category
ORDER BY default_rate DESC;

#Risk Rank from loan grade, default on file, and percent default
select 
loan_grade,
cb_person_default_on_file,
avg(loan_status) as Percent_Default,
rank() over(order by avg(loan_status) desc) as risk_rank
from credit_risk_dataset
group by loan_grade, cb_person_default_on_file
order by Percent_Default desc;
