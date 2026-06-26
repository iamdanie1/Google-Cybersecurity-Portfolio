# SQL Log Analysis: Applying Data Filters to Investigate Security Incidents

## Project Description
In this portfolio project, I leveraged relational database filtering techniques to investigate multi-vector security anomalies involving unauthorized user access attempts and asset patch management cycles. Operating within a structured database environment containing enterprise tracking metrics, I formulated optimized SQL queries using logical operators (`AND`, `OR`, `NOT`) and pattern-matching conditions (`LIKE`) to parse system records. By effectively filtering targeted subsets of transaction logs and personnel metadata, I isolated malicious authentication behavior originating outside authorized bounds and compiled comprehensive asset lists to guide critical system patches.

---

## Database Architecture Overview
The security investigation relied on auditing two primary tables inside the `organization` database schema:
1. `log_in_attempts`: Tracks real-time authentication events, storing transactional data variables including `event_id`, `username`, `login_date`, `login_time`, `country`, `ip_address`, and `success` (where `0` or `FALSE` signifies authentication failure).
2. `employees`: Tracks corporate physical asset mappings, detailing `employee_id`, `device_id`, `username`, `department`, and physical `office` assignments.

---

## Retrieve After-Hours Failed Login Attempts
A potential network entry anomaly occurred outside regular corporate operating parameters. To evaluate the scale of this access violation, I constructed a filter targeting unauthorized authentication sessions executed after business hours ($18:00$) that failed to validate successfully.

```sql
SELECT *
FROM log_in_attempts
WHERE login_time > '18:00:00' AND success = FALSE;
```
### Explanation of Query Logic

* The `SELECT` statement pulls core forensic metadata columns from the transactional `log_in_attempts` catalog.
* The `WHERE` clause applies a multi-conditional filter combining two distinct parameters using the logical `AND` operator:
1.`login_time > '18:00:00'`: Evaluates the timestamp string array to isolate traffic occurring explicitly after business hours.
2.`success = FALSE`: Targets exclusively failed authentication transactions (can alternately be evaluated using the integer token `0`).
* Because the `AND` operator evaluates conditionally, records are only returned if the event satisfies both criteria simultaneously.
## Retrieve Login Attempts on Specific Dates

A targeted attack vector was detected on the network environment on `2022-05-09`. To trace systemic preparatory tracking or follow-on activity, I queried authentication system attempts spanning both the target date and the preceding 24-hour cycle (`2022-05-08`).
```sql
SELECT *
FROM log_in_attempts
WHERE login_date = '2022-05-09' OR login_date = '2022-05-08';
```
### Explanation of Query Logic  
* To scan logs across a disparate timeline window, I utilized the logical `OR` operator within the filtering statement.
* The condition checks each log line individually: if either the value in `login_date matches '2022-05-09'` or it matches `'2022-05-08'`, that record is processed into the data output. This configuration permits the ingestion of events happening across either date threshold.

## Retrieve Login Attempts Outside of Mexico
Threat intelligence analysts confirmed that the ongoing adversarial authentication anomalies did not originate from corporate assets located inside Mexico. To thin out the log noise and optimize the dataset for further geolocation analysis, I filtered out all regional traffic tied to Mexico.
```sql
SELECT *
FROM log_in_attempts
WHERE NOT country LIKE 'MEX%';
```
### Explanation of Query Logic
* The regional data markers in the database capture Mexico using varying naming schemas (`MEX` and `MEXICO`). To account for formatting variances while applying an inversion, I combined the `NOT` operator with the wildcard pattern matching operator `LIKE`.
* The token `'MEX%'` utilizes the percentage sign (`%`) wildcard character, matching any string array that begins with the root letters M-E-X.
* Prepending the condition with the logical `NOT` operator reverses the logic matrix, forcing the database engine to isolate and return every entry except those originating within Mexico.

## Retrieve Employees in Marketing
The Security Operations Center (SOC) mandated urgent security baseline updates on endpoints deployed throughout the Marketing department, specifically targeting nodes physically stationed within the East office building structure.  
```sql
SELECT *
FROM employees
WHERE department = 'Marketing' AND office LIKE 'East%';
```

### Explanation of Query Logic
* This query addresses distinct infrastructure constraints across separate asset attributes using the `AND` operator to link parameters.
* `department = 'Marketing'` limits the query strictly to assets registered to marketing personnel.
* `office LIKE 'East%'` evaluates office strings (such as `East-170` or `East-320`). By passing the text pattern root along with the `% `wildcard character, the filter safely flags every endpoint in the target wing regardless of floor or specific room numbering variables.

* ## Retrieve Employees in Finance or Sales
* A critical software patch cycle was ordered specifically for employee machines allocated within either the Finance or Sales operational teams.
```sql
SELECT *
FROM employees
WHERE department = 'Finance' OR department = 'Sales';
```
### Explanation of Query Logic
*Because an employee can only be mapped to one primary department entry line inside a given row, using an `AND` statement here would return zero data matches.
Instead, utilizing the logical `OR` operator permits the structural validation of both target conditions. The query inspects rows sequentially, returning the employee listing if the target entry checks out as either `'Finance'` or `'Sales'`.  

## Retrieve All Employees Not in IT
The corporate network security team verified that endpoints assigned to the Information Technology (IT) department received a critical system configuration update. To identify the remainder of the corporate asset landscape requiring the same enforcement, I queried all non-IT department listings.  
```sql
SELECT *
FROM employees
WHERE NOT department = 'Information Technology';
```
### Explanation of Query Logic
* To target the inverse of a specific administrative sector, I applied the logical exclusion operator `NOT`.
* The condition evaluate evaluates the string field `department = 'Information Technology'`. By applying NOT, the filter instructs the relational database processing layer to skip all matching IT entries entirely, outputting the complete record listing for all other corporate teams requiring the upgrade path.
---

## Summary
Through this logical filtering initiative, I demonstrated how to successfully audit relational enterprise databases to support internal incident response and infrastructure vulnerability management workflows. By combining specific database parameters using Boolean filtering strings (`AND`, `OR`, `NOT`) and wildcards (`%`), I parsed high-volume log archives down to highly contextual, actionable alerts. My investigations isolated critical post-business-hour authentication anomalies, tracked threat execution timelines across historical windows, and excluded trusted regional traffic to surface potential brute-force vectors. Furthermore, by writing structured queries against asset metadata databases, I efficiently mapped non-compliant corporate endpoints across specific departments and physical locations, providing technical infrastructure teams with precise target inventories to systematically apply critical software updates and enforce corporate risk baselines.
