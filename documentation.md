# Tables

## engine_log
| Column Name | Type | Joins To | Notes |
|-------------|------|----------|-------|
| RPM | NUMERIC | | Engine revolutions per minute for vehicle performance tracking |
| MPH | NUMERIC | | Miles per hour for vehicle performance tracking |
| time | NUMERIC | | Elapsed time in seconds since the beginning of the log, not guaranteed to be unique |
| file_date | VARCHAR | | The date and time of the log file in 'YYYY-MM-DD_HH.MM.SS' format |
| MAT | DOUBLE | | Manifold Air Temperature reading |
| CLT | DOUBLE | | Current engine coolant temperature |
| MAP | DOUBLE | | Current Intake Manifold Pressure |
| Batt V | DOUBLE | | Current battery voltage |
| TPS | DOUBLE | | Engine's current throttle position as a percentage |
| AFR | DOUBLE | | Current Air Fuel Ratio reading from oxygen sensor |
| Barometer | DOUBLE | | Current barometric pressure reading |
| TPSdot | DOUBLE | | Rate of change of TPS (the increase in throttle position per second) |
| MAPdot | DOUBLE | | Rate of change of MAP (the increase in manifold pressure per second) |
| RPMdot | DOUBLE | | Rate of change of RPM (the increase in engine revolutions per minute per second) |
| PW | DOUBLE | | Fuel pulsewidth for injector channel 1 (the actual electrical pulsewidth including deadtime) |
| VE1 | DOUBLE | | Value looked up in the VE table |
| Accel PW | DOUBLE | | Current fuel pulsewidth adder due to acceleration enrichment |
| AFR load | DOUBLE | | Engine load value used on the Y-axis of the AFR table |
| Fuel: Total cor | DOUBLE | | Total fuel percentage multiplier obtained by multiplying other factors (values outside 80%-120% range may indicate issues) |
| Load | DOUBLE | | Primary load for fuel calculations (should match 'AFR load' value) |

## lambda_delay
| Column Name | Type | Joins To | Notes |
|-------------|------|----------|-------|
| load | UNKNOWN | | | 
| rpm | UNKNOWN | | | 
| delay_ms | UNKNOWN | | Time delay in milliseconds from when the engine fires until the exhaust gasses reach the O2 sensor to measure the air fuel ratio |

# Canonical Queries

# Important Notes for Querying

## SQL Dialect Notes
DuckDB does not support using the window function row_number() in the WHERE clause. When writing queries for DuckDB, avoid filtering directly on window functions in the WHERE clause and instead use subqueries or CTEs to materialize the window function results first.

# Facts

## Vehicle Performance Tracking
The engine_log table contains 105 columns covering engine performance, GPS, and vehicle dynamics data. Most columns are of DOUBLE type, with file_date being the only VARCHAR column.

The table tracks detailed engine metrics including RPM, MAP, Boost PSI, TPS, AFR, and various fuel corrections. It also includes spark-related columns tracking spark advance, retard, and correction factors.

For location tracking, the table captures GPS and location data including latitude, longitude, heading, and accuracy. Performance metrics include Zero to 60 Time, Power, Torque, and various force measurements.

The engine_log table contains multiple log files. The 'time' column represents elapsed time in seconds since the beginning of the log and is not guaranteed to be unique. The 'file_date' column stores the date and time of the log file in 'YYYY-MM-DD_HH.MM.SS' format.

Data in the engine_log table is generated from a four cylinder, four stroke gasoline 1608cc engine. This engine has fuel injectors that flow approximately 17.1 lb/hr. The computer producing the logs is a microsquirt ECU.

The vehicle uses a 5 speed manual transmission that drives the rear wheels through a 3.91:1 ratio differential.

The vehicle is equipped with 185/60 R 14 tires.

The stoic target for this engine is 14.7, which represents the ideal air-fuel ratio for optimal combustion efficiency.

The lambda_delay table captures the time delay from when the engine fires until the exhaust gasses reach the O2 sensor to measure the air fuel ratio.

## Engine Operating Conditions
The engine is considered at operating temperature when the coolant temperature (CLT) exceeds 168 degrees.

The engine is considered at idle when RPMs are around 1000 and the throttle position (TPS) is less than 2%.