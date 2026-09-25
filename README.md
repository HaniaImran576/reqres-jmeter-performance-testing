# ReqRes API Performance Testing with JMeter

A load and stress testing project on the ReqRes public API using Apache JMeter, measuring response time, throughput, and error rate across increasing levels of concurrent load, from a single user baseline up to 300 concurrent users.

## Objective
To establish a performance baseline for the ReqRes API and observe how response time, throughput, and error rate behave as concurrent load increases, up to a defined stress level.

## Test Levels

| Test Type | Concurrent Users | Average Response Time | Throughput | Error Rate |
|---|---|---|---|---|
| Baseline | 1 | 92 ms | 1.0 / sec | 0.00% |
| Load | 10 | 236 ms | 3.6 / sec | 0.00% |
| Load | 25 | 377 ms | 4.3 / sec | 0.00% |
| Load | 50 | 118 ms | 5.1 / sec | 0.00% |
| Stress | 100 | 119 ms | 9.1 / sec | 0.00% |
| Stress | 300 | 139 ms | 19.4 / sec | 0.00% |

## Key Findings
- Throughput scaled consistently with concurrent users, from 1.0 requests per second at baseline up to 19.4 requests per second at 300 concurrent users, confirming that load was generated and processed as expected at each level.
- Error rate remained at 0.00% across every test level, including the highest stress level of 300 concurrent users. No breaking point was reached within the scope of this test.
- Response time rose from the single user baseline of 92 ms to a peak of 377 ms at 25 concurrent users, then settled into a lower, stable range of approximately 118 to 139 ms from 50 users through 300 users. The dip and plateau after 25 users is worth noting rather than assuming away, and is most likely explained by connection reuse or pooling stabilizing once enough concurrent traffic was underway. Since each test level was run as a single iteration, repeating each level multiple times in future testing would help confirm whether this pattern holds consistently.

## No Breaking Point Identified
Unlike an earlier finding in this project's related manual and API testing work, where ReqRes's free tier rate limiting was triggered during rapid sequential requests in Postman, this JMeter test did not encounter rate limiting or any errors up to 300 concurrent users. This suggests the two testing approaches exercised the API differently, likely due to differences in request timing and concurrency pattern between a Postman Collection Runner sequence and JMeter's thread based concurrent execution. The true breaking point for concurrent load, as opposed to rapid sequential requests, was not reached within this test's scope and would require testing at higher concurrency levels to identify.

## Tools
- Apache JMeter, for test plan design and load generation
- JMeter Aggregate Report and Summary Report listeners, for results analysis

## Test Plan Structure
- Thread Group configured per test level (1, 10, 25, 50, 100, and 300 threads)
- HTTP Request sampler targeting ReqRes API endpoints
- Aggregate Report and Summary Report listeners for response time, throughput, and error rate metrics

## How to Run
1. Open the `.jmx` test plan file in Apache JMeter.
2. Adjust the Thread Group's number of threads to the desired concurrency level.
3. Run the test and review results in the Aggregate Report or Summary Report listener.
4. Export results as CSV or generate an HTML dashboard report using JMeter's built in reporting for further analysis.

## Related Work
This project extends earlier API testing on ReqRes, which covered functional and negative test cases using Postman, including CRUD operations, validation testing, and an earlier observed rate limiting constraint on rapid sequential requests.

## Next Steps
- Test concurrency levels beyond 300 users to determine whether a genuine breaking point exists under sustained concurrent load.
- Repeat each test level across multiple iterations to confirm whether the response time dip observed between 25 and 50 users is consistent or a result of natural single run variance.
- Add a sustained endurance test at a moderate concurrency level to check for performance degradation over a longer duration.
