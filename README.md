# AIOps Service Monitoring Assessment

## AIOps Scenario

This project is about monitoring a `payment-service`. The service data contains response time, CPU usage, memory usage and log details.

The aim is to find abnormal behaviour in the service and pass those anomalies through the given AIOps workflow.

## Operational Data

The data is stored in `data/service_data.json`.

It contains:

- timestamp
- service
- response time
- CPU usage
- memory usage
- log level
- message

Most of the records are normal.

The main unusual records are:

- `10:05` - response time `610 ms` with an `ERROR` log for a payment service timeout.
- `10:06` - response time `640 ms`, CPU `94%`, memory `91%` and an `ERROR` log for a database connection timeout.

## Anomaly Detection

The detector checks response time, CPU and memory against fixed limits.

It detected 2 anomalies:

- `10:05` - High response time, Error log detected
- `10:06` - High response time, High CPU utilization, High memory utilization, Error log detected

No expected anomaly was missed in the provided data, and no normal event was incorrectly flagged.

## Event Flow

The flow used in the project is:

`Data → Anomaly Detection → Event → Producer → Topic → Consumer → AIOps Output`

The project uses an in-memory topic for the event flow.

## Issues Found

Two issues were found in the provided code.

1. The detector was checking for `WARNING`, but the data contains `ERROR`. This was changed to check for `ERROR`.

2. The producer and consumer were using different topics. The consumer was changed to use the same topic as the producer.

After these changes, both anomaly events were received by the consumer.

## Final Result

The final run processed 10 records.

2 anomalies were detected and 2 events were consumed successfully.

The final output showed both anomaly events with their reasons.

## Limitation

The detector uses fixed thresholds, so changing the threshold values can change the anomaly results.

## Run

From the repository root:

```bash
python src/aiops_pipeline.py