
# Estimating Care Plan Contributions to Daily Care Demand

**Author:** Chamberlain Mbah
**Date:** \today

## Problem Description

The goal is to estimate the contribution of each care plan to the overall daily care demand in different hospital departments. This involves translating the problem into a regression framework to predict the daily care demand based on the occurrences of various care plans.

## Assumptions

To simplify the analysis, we make the following assumptions:

- The care supply and care demand are in balance, meaning the total care supply equals the total care demand on any given day.
- The time required to administer a care plan to a patient is constant and does not vary between patients.
- We will only consider the sum of all care plan occurrences on a ward on a given day, ignoring the number and heterogeneity of patients.
- Zero care supply days and outliers (determined using the Interquartile Range (IQR) method) are excluded from the analysis.

## Data Structure

The data consists of:

- **day:** The specific day of the observation.
- **care_plan:** The identifier for the specific care plan.
- **occurrences:** The number of times the care plan was administered on that day.
- **total_dpt_supply_in_minutes:** The total care supply in minutes for that day.
- **department_name:** The name of the department.

## Deriving Care Demand

Given the assumption that care supply equals care demand on any given day, we directly use the total care supply values as our measure of care demand. This means that the daily care demand for each department is derived from the recorded total care supply in minutes. Thus, the care demand \( D_i \) for day \( i \) is given by:

\[ D_i = \text{total\_dpt\_supply\_in\_minutes} \]

This straightforward approach allows us to leverage the available supply data to estimate care demand without additional transformations or assumptions.

## Methodology

The problem is translated into a regression framework where the daily care demand is predicted based on the occurrences of various care plans. We assume each care plan contributes independently to the daily care demand.

### Mathematical Formulation

Let:

- \( n \) be the number of days.
- \( m \) be the number of different care plans.
- \( O_{i,j} \) be the number of occurrences of care plan \( j \) on day \( i \).
- \( D_i \) be the total care demand (or supply) for day \( i \).
- \( d_j \) be the contribution of care plan \( j \) to the daily care demand.

The relationship between the total daily care demand and the occurrences of care plans can be expressed as:

\[ D_i = \sum_{j=1}^{m} O_{i,j} \cdot d_j \]

In matrix form:

\[ \mathbf{D} = \mathbf{O} \cdot \mathbf{d} \]

where:

- \( \mathbf{D} \) is an \( n \times 1 \) vector of daily care demand values.
- \( \mathbf{O} \) is an \( n \times m \) matrix of care plan occurrences.
- \( \mathbf{d} \) is an \( m \times 1 \) vector of unknown care plan contributions.

To solve for \( \mathbf{d} \), we use linear regression, where the coefficients \( \mathbf{d} \) are estimated by minimizing the sum of squared residuals:

\[ \mathbf{d} = (\mathbf{O}^T \mathbf{O})^{-1} \mathbf{O}^T \mathbf{D} \]

### Implementation

In practice, we fit individual simple linear regressions for each care plan to estimate their contributions independently. This simplifies the computation and interpretation of the model. We perform this analysis separately for each department to account for potential differences in care practices and resource allocation.

## Data Cleaning

We eliminate days with zero care supply and identify outliers using the Interquartile Range (IQR) method. Outliers are defined as values that lie beyond 1.5 times the IQR above the third quartile or below the first quartile.

## Next Steps

Here are some thoughtful next steps to enhance the analysis and ensure its robustness:

- **Cross-Validation:** Implement cross-validation to assess the model's predictive performance and ensure it generalizes well to unseen data.
- **Refine Data Cleaning:** Investigate the root causes of unknown care plans and outliers. Consider whether these can be reclassified or if additional context can be provided to improve data quality.
- **Domain Expertise:** Engage with healthcare professionals to validate the estimated care plan contributions and gather insights on any discrepancies.
- **Longitudinal Analysis:** Analyze trends over time to identify any seasonal or systematic variations in care demand and supply.
- **Integration with Hospital Systems:** Use the estimated contributions to enhance hospital planning and resource allocation by integrating the results into existing hospital management software.
- **Expand Scope:** Consider extending the analysis to account for patient heterogeneity and other factors that might affect care demand, such as patient demographics or severity of illness.

## Conclusion

This methodology allows us to estimate the contribution of each care plan to the total daily care demand. The estimated contributions can then be used to predict future care demand based on the occurrences of care plans. By refining the model and incorporating domain expertise, we can provide valuable insights to improve hospital resource management.


