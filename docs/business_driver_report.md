# Business Driver Report

## Why Customers Churn
The elevated model shows churn is not a single-factor event. Risk rises when lifecycle timing, service friction, billing pressure, and value signals combine. Customers with aging devices, frequent care interactions, overage pressure, and weaker revenue efficiency are more likely to become retention priorities.

## Which Features Matter Most
- `num__change_mou`: importance `2159`
- `num__totmrc_Mean`: importance `1632`
- `num__revenue_efficiency`: importance `1609`
- `num__change_rev`: importance `1575`
- `num__mou_Mean`: importance `1214`
- `num__eqpdays_per_month`: importance `1147`
- `num__avgqty`: importance `1053`
- `num__months`: importance `1043`
- `num__avgrev`: importance `1014`
- `num__revenue_per_phone`: importance `967`
- `num__mou_cvce_Mean`: importance `950`
- `num__avgmou`: importance `940`

## Which Features Are Actionable
- `eqpdays`, `eqpdays_per_month`, and `device_age_relative`: actionable through upgrade eligibility, handset refresh, and proactive renewal messaging.
- `custcare_Mean`, `custcare_per_month`, and `device_age_customer_care`: actionable through service recovery, escalation, and targeted support follow-up.
- `ovrmou_Mean`, `overage_per_month`, and `overage_tenure`: actionable through plan right-sizing, overage alerts, and fee relief.
- `rev_Mean`, `revenue_per_phone`, `revenue_efficiency`, and `customer_value_score`: actionable for prioritizing scarce retention budget.
- `kmeans_segment` and `gmm_segment`: actionable for campaign segmentation and differentiated intervention design.

## Recommended Interventions
- Device-age and tenure interactions indicate upgrade timing should be part of retention targeting.
- Customer-care intensity and care-per-month features translate directly into service recovery actions.
- Overage pressure features support bill-shock prevention, plan migration, or fee-relief offers.
- Revenue and customer-value features make the action list economically ranked rather than purely risk ranked.
- KMeans and Gaussian Mixture segments give campaign teams discrete cohorts for messaging and offer design.

## Executive Interpretation
The most useful retention action is not simply calling every high-risk customer. The model should rank customers by churn probability and expected value, then match the intervention to the reason for risk: upgrade for lifecycle risk, service recovery for support friction, plan migration for overage pressure, and concierge outreach for high-value customers.
