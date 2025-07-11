

# DeepTrace

Execution Journey
Initially, I faced challenges in understanding the Selenium framework and the Java-based Page Object Model used for automating regression testing. The codebase was extensive and complex, making it difficult to comprehend at first.

To overcome this, I referred to:

Customer Website Integration (CWS) documentation to understand the logic and flow

LinkedIn Learning courses

Selenium’s official documentation

These resources helped me gain clarity on the file structure and the overall flow of the automation framework.

Knowledge Transfer & Business Flow Understanding
Through knowledge transfer sessions, I gained a thorough understanding of:

Loan Apply (LA) Journey: A token-based flow that starts from the Loan Details page and ends with the Loan Application Submission Acknowledgement page.

Loan Cross Sell (LXS) Flow: A subset of the loan apply journey that tests the cross-sell loan scenarios.

CWS Flow: The complete customer website journey starting from login, transitioning through the loan flow, and concluding with the final submission acknowledgement.

Bug Fixes and Flow Validations
Fixed the following LA (Loan Apply) flows:

With Rewards and Without Add Bank Account: The journey where the customer has rewards and selects the bank accounts that are already stored in the database using a dropdown.

Without Rewards and With Add Bank Account: The journey where the customer does not have rewards and adds a new bank account manually.

Fixed the following CWS flows:

Without Rewards and Without Add Bank Account: The journey starts from customer login, transitions into the loan journey where reward validation is skipped, and the user selects an existing bank account.

With Rewards and With Add Bank Account: The journey starts from customer login, reward validation occurs, and the user adds a new bank account during the loan application process.

Enhancements and Utilities Added
FAQ Interactions and Validations: Implemented functionality to validate the visibility and expand/collapse behavior of the FAQ section at the bottom of each page. Ensured smooth interaction by verifying click responses and dropdown content.

Utility to Find Text on Page: Created a reusable utility to locate specific text on a page, helpful for assertion checks and improving test coverage in regression suites.

View Agreement Functionality: Added automated validation for the "View Agreement" section on the Loan Summary page, ensuring the content loads correctly and the user interaction is smooth.

Key Learnings from Mentorship and Project Walkthroughs
Collaborating closely with senior team members provided valuable hands-on learning and helped accelerate my understanding of the project. Key learnings include:

Usage of Postman: Learned to generate and manage tokens for token-based flows, ensuring proper authentication during automated test execution.

Version Control Best Practices: Understood how to use Bitbucket effectively, including branching strategies, commit hygiene, and pull request reviews.

Loan Apply Architecture: Gained a detailed explanation of the internal working of the Loan Apply journey, including request-response lifecycle, token validation, and navigation flow.

Environment-Specific Properties: Learned the structure and usage of different environments (e.g., Dev, QA, UAT), and how their respective property files impact test execution.

Test Data Preparation: Understood how to create and maintain sample data for consistent testing, especially for edge cases and negative scenarios.

Account Status Validation: Learned how to verify account status and eligibility criteria before and after each step in the loan flow.

Error Debugging Using Logs: Understood how to trace and debug issues using application logs, console output, and test failure traces to identify root causes quickly.


Learnings and Takeaways
This project offered a steep learning curve and helped me grow both technically and professionally. Key takeaways include:

Understanding of Selenium and Test Automation

learned how to use Selenium's fundamental features, such as assertions, wait strategies, handling dynamic elements, and reusable page objects. My ability to create scalable and reliable test cases improved as a result.


Importance of Environment Awareness
understood how property files, APIs, and test data differ between various environments (Dev, QA, and UAT) and how they behave differently. This made it easier to configure and run tests more consistently.

Effective Debugging and Root Cause Analysis
learned how to analyze server and application logs to determine the underlying reason why tests fail. My turnaround time for resolving bugs and clearing the test pipeline was greatly accelerated by this.

Cross-Team Communication
Regularly interacted with developers and QA leads to clarify business logic, understand code-level dependencies, and troubleshoot technical issues. These discussions helped me align test scenarios with real-world use cases and resolve blockers efficiently.

Postman and API Testing
enhanced my knowledge of REST API testing with Postman and token-based authentication. learned how to troubleshoot issues in API-driven workflows, validate responses, and chain requests.

Version Control Discipline
Adopted best practices in Git/Bitbucket, including branch naming conventions, atomic commits, and detailed PR reviews, leading to cleaner collaboration and safer code deployments.
