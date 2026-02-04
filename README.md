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
