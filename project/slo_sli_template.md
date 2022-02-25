# API Service

| Category     | SLI | SLO                                                                                                         |
|--------------|-----|-------------------------------------------------------------------------------------------------------------|



Availability
sum (rate(apiserver_request_total{job="apiserver",code=~"2.."}[5m])) / sum (rate(apiserver_request_total{job="apiserver"}[5m])) 
99% of successful 200 web requests per 5 minute interval   



Latency
histogram_quantile(0.90, sum(rate(apiserver_request_duration_seconds_bucket{job="apiserver"}[5m])) by (le, verb))    
90% of requests below 100ms
Buckets of requests showing the 90th percentile over the last 5 minutes



Error Budget
1 - ((1 - (sum(increase(apiserver_request_total{job="apiserver", code="200"}[5m])) by (verb)) /  sum(increase(apiserver_request_total{job="apiserver"}[5m])) by (verb)) / (1 - .80))
Error budget is defined at 20%. 
This means that 20% of the requests can fail and still be within the budget
80% of requests are successful within a 5 minute window - From the Training slides, noramlly something larger is used, a five or seven day window.



Throughput
sum(rate(apiserver_request_total{job="apiserver",code=~"2.."}[5m]))
5 RPS indicates the application is functioning
Total number of 'successful' requests per second over 5 minute interval
