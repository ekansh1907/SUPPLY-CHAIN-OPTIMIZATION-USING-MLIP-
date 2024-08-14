# Supply Chain Optimization Using MLIP

This repository contains the implementation of a project titled "Supply Chain Optimization Using MLIP." The project focuses on optimizing the supply chain across multiple dimensions using Machine Learning and Integrated Business Planning (IBP). 

## Project Overview

### Major Initiatives
- **Rolling Procurement, Production & Logistics Plan:** Implemented a 2-month rolling plan leveraging IBP to optimize the supply chain.
- **Multi-Modal Optimization:** Managed 500+ routes via rail, road, and sea across 2 plants, 50+ depots, 5+ rake terminals, and 3 ports.

### Project Achievements
- Vendor Prioritization: Enabled prioritization of over 2500 vendor-SKUs based on procurement cost and transportation lead time.
- Enhanced Demand & Inventory Fulfillment: Planned for 200+ Finished Goods (FGs) and 50+ Semi-Finished Goods (SFGs), resulting in an increase in customer demand and inventory fulfillment from 55.7% to 98.32% QoQ.

### Supply Chain KPIs
- Efficiency in IBP:
  - Procurement: Achieved 76.3% efficiency (Planned vs Actuals)
  - Production: Achieved 88.2% efficiency (Planned vs Actuals)
  - Logistics: Achieved 71.5% efficiency (Planned vs Actuals)
- Revenue Growth: Performed requirement vs commit vs actuals analysis for demand fulfillment, resulting in a 4.6% increase in revenue targets YoY.

## Repository Structure

- `code/`: Contains the R Studio code used for the project.
- `data/`: Contains the random dataset used for the project in Excel format.
- `README.md`: Project overview and instructions.

## Getting Started

### Prerequisites
- R Studio
- Required R packages: `dplyr`, `ggplot2`, `forecast`, `caret`, `readxl`

### Running the Code
1. Clone the repository.
   ```bash
   git clone https://github.com/yourusername/supply-chain-optimization-mlip.git



---

### R Studio Code

```r
# Supply Chain Optimization Using MLIP

# Load required libraries
library(dplyr)
library(ggplot2)
library(forecast)
library(caret)
library(readxl)

# Load the dataset
data <- read_excel("data/supply_chain_data.xlsx")

# Data Preprocessing
data_clean <- data %>%
  filter(!is.na(Procurement_Cost), !is.na(Lead_Time))

# Vendor Prioritization based on Procurement Cost & Lead Time
vendor_priority <- data_clean %>%
  group_by(Vendor_SKU) %>%
  summarise(
    Avg_Cost = mean(Procurement_Cost),
    Avg_Lead_Time = mean(Lead_Time)
  ) %>%
  arrange(Avg_Cost, Avg_Lead_Time)

# Plot Vendor Prioritization
ggplot(vendor_priority, aes(x = Avg_Cost, y = Avg_Lead_Time)) +
  geom_point() +
  labs(title = "Vendor Prioritization",
       x = "Average Procurement Cost",
       y = "Average Lead Time")

# Demand Fulfillment Analysis
demand_analysis <- data %>%
  group_by(Product) %>%
  summarise(
    Planned_Demand = sum(Planned_Demand),
    Actual_Demand = sum(Actual_Demand),
    Demand_Fulfillment = sum(Actual_Demand) / sum(Planned_Demand) * 100
  )

# Plot Demand Fulfillment
ggplot(demand_analysis, aes(x = Product, y = Demand_Fulfillment)) +
  geom_bar(stat = "identity") +
  labs(title = "Demand Fulfillment by Product",
       x = "Product",
       y = "Demand Fulfillment (%)") +
  coord_flip()

# Supply Chain KPIs
kpi_analysis <- data %>%
  summarise(
    Procurement_Efficiency = mean(Planned_Procurement / Actual_Procurement * 100),
    Production_Efficiency = mean(Planned_Production / Actual_Production * 100),
    Logistics_Efficiency = mean(Planned_Logistics / Actual_Logistics * 100)
  )

print(kpi_analysis)

# Forecasting future demand (example using ARIMA)
future_demand <- auto.arima(data$Actual_Demand)
forecasted_demand <- forecast(future_demand, h = 12)

plot(forecasted_demand)
