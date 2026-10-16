# SDET Assignment - Initial Understanding and Approach

## 1. Objective
To validate the reliability and correctness of the Subscription and Billing Service using end-to-end API testing and database validation where access is available.

## 2. Test Coverage
- API response and contract validation
- Subscription and billing workflows
- Positive and negative scenarios
- Duplicate requests and idempotency
- Retry handling and failure recovery
- Partial failures and transactional consistency
- API response and database state consistency

## 3. Automation Approach
Use JavaScript and Playwright API testing. Organize tests by feature and workflow, with reusable helpers and maintainable test data.

## 4. Data Validation
Verify that successful operations produce the expected database state. Validate that failed or retried operations do not create unintended duplicate records or inconsistent data.

## 5. Assumptions and Limitations
The final test design will follow the provided assignment requirements and available API and database access. Any assumptions or access limitations will be documented.

## 6. Next Steps
Review the requirements, finalize test scenarios, implement automation, execute tests, and document the results.

This is the initial approach for review before implementing the complete solution.
