# Surgical-Supply-Chain-Analysis
# Healthcare Supply Chain and Risk Analysis
## Project Overview
**Objective:** Evaluate surgical supplier lead times, optimize regional order allocation, and quantify vendor risk levels.

**Key Findings:**

*Discovered critical lead-time variances across high-volume surgical SKUs.
  
*Identified regional order volume imbalances causing fulfillment inefficiencies.

*Highlighted a multi-factor risk profile to categorize supplier dependability.

**Business Impact**: Mitigates the risk of surgical stock-outs, reduces supply waste from expired inventory, and eliminates unnecessary spending on freight costs.

---
## Project Links
•	**Tableau** [Interactive Tableau Dashboard] ( https://public.tableau.com/app/profile/roseann.bell/viz/HealthcareSupplyChainRisks/Dashboard1)

•	**Dataset** [Kaggle Supply Chain Dataset (1400+ transactional records)(https://public.tableau.com/app/profile/roseann.bell/viz/HealthcareSupplyChainRisks/Dashboard1)

---
## Business Questions Addressed
*What is the average lead time for surgical SKUs grouped by supplier?

*Which region orders the lowest total volume of surgical SKUs and how many distinct orders were placed?

*How many High, Medium, and Low risk transactions is each supplier associated with?


---
## Technical Tool Stack

	*Bigquery SQL: Queried relational data to calculate order aggregates, quantify unit totals, and identify supplier dependencies.
  
	*Excel: Conducted initial data auditing, inspected raw Kaggle data for missing values/formatting consistency, and created preliminary exploratory visuals.
  
	*Tableau Public: Built the final, interactive executive dashboard, applying custom risk color coding, standardized layout containers, and dynamic formatting.
---
## Data Analysis and SQL Queries
### Question 1: What is the average lead time for surgical SKUs grouped by supplier?

```sql
SELECT AVG(lead_time) AS avg_lead_time, supplier
FROM `supply-chain-project-507003.inventory_transactions.inventory_transactions` AS i
JOIN `supply-chain-project-507003.Product_master_list.Product_master_list` AS p
	 ON i.SKU = p.SKU
WHERE category LIKE "%Surgical%"
GROUP BY Supplier
```

### Question 2: Which region orders the lowest total volume of surgical SKUs and how many distinct orders were placed?

```sql
SELECT location,
  	SUM(Qty_sold) AS total_qty_sold,
   	COUNT (DISTINCT order_id) AS distinct_orders
FROM `supply-chain-project-507003.inventory_transactions.inventory_transactions` AS i
JOIN `supply-chain-project-507003.Product_master_list.Product_master_list` AS p
  	ON i.SKU = p.SKU
WHERE category LIKE "%Surgical%"
GROUP BY location
ORDER BY total_qty_sold ASC
```

## Question 3: How many High, Medium, and Low risk transactions is each supplier associated with?

```sql
SELECT p.supplier,
      	COUNTIF(i.Risk_level = "High Risk") AS High_Risk_Count,
      	COUNTIF(i.Risk_level = "Medium Risk") AS Medium_Risk_Count,
        COUNTIF(i.Risk_level = "Low Risk") AS Low_Risk_Count,
FROM `supply-chain-project-507003.inventory_transactions.inventory_transactions`  AS i
JOIN `supply-chain-project-507003.Product_master_list.Product_master_list` AS p
  	ON i.SKU = p.SKU
GROUP BY p.Supplier
ORDER BY High_Risk_Count DESC;
 ```
---
## Key Insights and Recommendations
**Surgical Lead Time**: Identified an average lead time of 37.29 Days, heavily reliant on a single supplier (MedEye Tech) for surgical supplies. Relying on a sole supplier for all surgical SKUs has created operational dependency that poses a high risk for the supply chain in the event the supplier encounters any operational issues on their end. It is recommended that leadership consider contracting with a secondary supplier for surgical supplies needed. Leadership may also want to order buffer stock of the supplies with the most frequent usage to help prevent stockouts of those specific supplies.

**Regional Volume**: The East region recorded the lowest volume of surgical SKU orders of all regions (86 distinct orders). Although the East and North regions both tied for the lowest distinct orders, the East region had the lowest total units sold of the two regions (1,166). A large portion of surgical inventory in this warehouse could be expiring because too few orders are coming from this warehouse. There could be potential cost-saving opportunities here. The surgical orders from this warehouse could potentially be consolidated with one of the other warehouses already in use. Thus, eliminating the freight fees associated with this warehouse.

**Supplier Risk Profile**: The supplier MedEye Tech accounts for 100% of the high-risk category by transaction (192). This finding is highlighted further because this supplier is also the sole supplier of surgical supplies. Lower risk categories are highlighted by suppliers ClearVision and AquaDrop. This distinguishes MedEye Tech as the primary supply chain liability. Leadership should immediately reach out to MedEye Tech and investigate why this supplier is the high-risk leader and work with them on a performance improvement plan to mitigate the risks. Leadership should also consider sourcing new suppliers if the high risk level for MedEye Tech is unable to be mitigated.

---
## Data Security and Governance
To protect intellectual property and underlying raw data, the live Tableau visualization is hosted with workbook/data download permissions strictly disabled.

