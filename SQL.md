# Figure 3.0: High-Throughput Normalized Entity-Relationship Diagram & SQL Optimization Engine
**Portfolio Module: High-Performance Data Lakehouse Calculations**
**Architecture Archetype: Kappa Streaming Engine**

---

## Section 1: Average Daily Consumption Per Meter
This query calculates the arithmetic mean consumption of electricity grouped by individual smart meter devices to establish core household baselines.

```sql
SELECT 
    meter_id,
    AVG(usage_kwh) AS average_daily_consumption_kwh
FROM 
    Readings
GROUP BY 
    meter_id
ORDER BY 
    average_daily_consumption_kwh DESC;
```

---

## Section 2: Standard Deviation of Usage Per Meter
This query measures consumption variance per device, which serves as the underlying statistical calculation used to trigger real-time theft alerts and automated field technician dispatches.

```sql
SELECT 
    meter_id,
    STDDEV(usage_kwh) AS consumption_standard_deviation,
    COUNT(reading_id) AS total_readings_evaluated
FROM 
    Readings
GROUP BY 
    meter_id
HAVING 
    COUNT(reading_id) > 1;
```

---

## Section 3: Point-in-Time Temporal Join (Historical Pricing Changes)
This script satisfies the core structural requirement to map daily consumption to its exact point-in-time timeline pricing contract row using slowly changing dimension effective date windows.

```sql
SELECT 
    m.hashed_customer_id,
    r.meter_id,
    r.reading_date,
    r.usage_kwh,
    t.tariff_id,
    t.price_per_kwh,
    (r.usage_kwh * t.price_per_kwh) AS calculated_daily_cost
FROM 
    Readings r
JOIN 
    Meters m ON r.meter_id = m.meter_id
JOIN 
    Tariffs t ON m.active_tariff_id = t.tariff_id
WHERE 
    r.reading_date >= t.effective_start 
    AND (r.reading_date <= t.effective_end OR t.effective_end IS NULL);
```

---

## Section 4: Late-Data Arrival Tracking & Incorrectly Billed Invoices
This advanced analytics query flags records where late device transmissions bypassed active contract windows, resulting in an automated delta calculation error flagged for reconciliation.

```sql
SELECT 
    r.reading_id,
    r.meter_id,
    r.reading_date,
    r.timestamp_received,
    COALESCE(t.price_per_kwh, 0.1500) AS applied_billed_price,
    actual_t.price_per_kwh AS correct_historical_price,
    (COALESCE(t.price_per_kwh, 0.1500) - actual_t.price_per_kwh) AS pricing_delta_error
FROM 
    Readings r
JOIN 
    Meters m ON r.meter_id = m.meter_id
LEFT JOIN 
    Tariffs t ON m.active_tariff_id = t.tariff_id
JOIN 
    Tariffs actual_t ON m.active_tariff_id = actual_t.tariff_id
WHERE 
    r.reading_date >= actual_t.effective_start
    AND (r.reading_date <= actual_t.effective_end OR actual_t.effective_end IS NULL)
    AND COALESCE(t.price_per_kwh, 0.1500) <> actual_t.price_per_kwh
    AND r.timestamp_received > (r.reading_date + INTERVAL '1' DAY);
```
(r.reading_date + INTERVAL '1' DAY);
```
