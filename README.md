# Credit-Risk-and-Loan-Approval-Analysis
Address asymmetric gaps between lenders and borrowers to predict probability of default
select loan_intent, 
count(*) as total_loan,
sum(loan_status) as Loan_Default,
avg(loan_status) as Percent_of_Default,
round(stddev(loan_status), 3) as Standard_Deviation
from credit_risk_dataset
group by loan_intent
order by Percent_of_Default desc;
