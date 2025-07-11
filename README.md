

# DeepTrace

Adapting to the Selenium Framework and Environment Setup
Initially, it was challenging to understand the existing Selenium framework and Java Page Object Model due to its complexity and layered structure. Setting up the test environment with the right configurations across Dev, QA, and UAT also involved resolving several compatibility and dependency issues.

103 Call Issue (Xperian External API Dependency)
One major hurdle was the recurring failure of the 103 API call, which was directed to an external service called Xperian. Since this was an external dependency, failures were hard to debug and often blocked progress. The resolution required close coordination with backend teams and understanding request-response patterns and fallback mechanisms.

RFT (Regression Functional Testing) Validation Failures
Several automated test flows were blocked due to RFT validation issues. These validations required accurate sample data, which was not always available or aligned with the test environment. This led to failed assertions and halted further execution until data preparation and validation logic were updated.

Environment Inconsistencies and Property Mismatches
Property files differed across environments, leading to token generation issues, broken links, and UI mismatches during test runs. Understanding the mapping between environments and their configurations was essential to stabilizing the automation suite.

Real-World Deployment Inconsistencies and Challenges
Worked in a volatile environment with frequent deployments, changing APIs, and inconsistent configurations, which impacted test stability and required quick adaptation and effective coordination.

AD Group and Configuration Alignment
Initially faced access issues due to incorrect AD group mappings and misconfigured permissions, which impacted test execution. Resolved the problem by identifying the right AD groups and ensuring proper configuration alignment across environments.
