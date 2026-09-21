# AIOps Service Monitoring Assessment

## AIOps Scenario

This project monitors a synthetic `payment-service` using operational data such as response time, CPU usage, memory usage, and log information.

The purpose is to detect abnormal service behaviour and process the detected anomalies through the provided AIOps pipeline.

## Operational Data

The data contains:

- `timestamp`
- `service`
- `response_time_ms`
- `cpu_percent`
- `memory_percent`
- `log_level`
- `message`

Most records show normal behaviour with low response time, CPU and memory usage.

The records at `10:05` and `10:06` show unusual behaviour.

At `10:05`:
- Response time: `610 ms`
- CPU: `75%`
- Memory: `70%`
- Log level: `ERROR`
- Message: `Payment service timeout`

At `10:06`:
- Response time: `640 ms`
- CPU: `94%`
- Memory: `91%`
- Log level: `ERROR`
- Message: `Database connection timeout`

## Anomaly Detection

The provided detector uses these thresholds:

- Response time: `500 ms`
- CPU: `80%`
- Memory: `80%`

It checks the operational metrics and log level to identify anomalies.

## Event Flow

The provided components follow this flow:

`Operational Data → Anomaly Detection → Event → Producer → Topic → Consumer → AIOps Output`

- `anomaly_detector.py` detects anomalies.
- `event_producer.py` publishes anomaly events.
- `event_topic.py` stores events in memory.
- `event_consumer.py` consumes events.
- `aiops_pipeline.py` runs the workflow.

## Issues and Corrections

The intentional issues found during testing and their corrections will be documented here.

## Final Result

The final pipeline execution result will be documented after testing.

## Limitation

The anomaly detector uses fixed thresholds, so its results depend on the configured threshold values.

## Reproduction

From the repository root:

```bash
python src/aiops_pipeline.py