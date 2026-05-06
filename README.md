# n8n-delivery-weather-workflow

An N8N workflow that consults weather conditions before a delivery and uses an AI step to classify feasibility with contextual reasoning.

<img width="1917" height="867" alt="image" src="https://github.com/user-attachments/assets/bc51ae1d-568d-49b7-aa77-7ba472844b30" />

## The Problem

Rescheduling a delivery last minute costs more than planning ahead. When you have multiple stops in a day and your vehicle has no weather protection, a sudden rainstorm means a wasted trip — and a frustrated customer.

This workflow tackles that before the delivery even starts.

## How It Works

1. The delivery responsible fills out a form with: city, delivery date, period (morning / afternoon / evening), and product type.
2. The workflow calls the **OpenWeather API** to fetch weather data for that city and date.
3. The weather data is passed to an **AI step**, which analyzes the conditions in context: temperature, humidity, wind speed, cloud cover, and precipitation forecast.
4. The AI step outputs a structured classification:
   - **Feasible** — conditions are acceptable for the delivery.
   - **Conditional** — some risk, worth monitoring.
   - **Infeasible** — high risk, recommend rescheduling or adapting the vehicle.
5. The result is presented back in the form, with full reasoning and a weather summary.
6. The human makes the **final decision**: approve, monitor, or reject.

The workflow surfaces the risk. The person decides.

## Example Output

```
Classification: Infeasible

Forecast of heavy rain in São Paulo on 08/05 in the afternoon,
17°C, 95% humidity, wind gusts of 8 m/s.
High risk for unprotected electronics.

Recommendation: reschedule the delivery or adapt the vehicle
for protected transport.
```

## Stack

- **N8N** — workflow orchestration and form interface
- **OpenWeather API** — weather data by city and date
- **LLM (AI step)** — contextual classification and structured output generation

## Form Fields

| Field | Type |
|---|---|
| City | Text |
| Delivery Date | Date |
| Delivery Period | Select (Morning / Afternoon / Evening) |
| Product | Text |

## Why N8N

N8N was the choice for building this MVP quickly and efficiently — no infrastructure code, no boilerplate, just connecting the pieces visually and iterating fast.

It made it straightforward to connect the form, the weather API, and the AI step in a single workflow. The visual flow also makes it easy to adjust the classification prompt or swap the weather provider later.

The interesting part was seeing the AI step adapt its reasoning to each specific scenario — not just checking thresholds, but generating a recommendation grounded in the product type and delivery period.

## Decision Flow

```
Form Input
    ↓
OpenWeather API
    ↓
AI Step (classify + reason)
    ↓
Structured Result (classification + reasoning + weather summary)
    ↓
Human Decision (approve / conditional / reject)
```

## Notes

- The AI classification is advisory only. The final call always belongs to the person responsible for the delivery.
- Weather data accuracy depends on OpenWeather coverage for the selected city.
- The prompt can be tuned to add product-specific rules (e.g., stricter thresholds for perishables or electronics).

## Form 

![Company Registration Form](https://github.com/user-attachments/assets/3d59c941-6067-4299-b323-81f5359bbd2c)

![Options Selection State](https://github.com/user-attachments/assets/ff5504a1-84dc-4c79-973f-8f7cf6c59e63)

![Final Result / Filled Form](https://github.com/user-attachments/assets/80cdc719-3633-4a2f-abcb-1e155fd951b5)
