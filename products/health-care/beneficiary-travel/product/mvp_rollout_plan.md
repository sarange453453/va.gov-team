
# Release Plan - BTSSS Travel Pay Status Page MVP

---
## Step 1: Development
Feature Toggles

| Toggle name | Description |
| ----------- | ----------- |
| [travel_pay_power_switch](https://api.va.gov/flipper/features/travel_pay_power_switch) | Main switch for the Travel Pay feature on VA.gov using the new BTSSS (travel pay) API. Enabled - Requests are handled as normal. Disabled - Requests are not handled. Server returns a 503 (Service Unavailable) until re-enabled. |

## Step 2: Validation

Since we use a [continuous delivery](https://depo-platform-documentation.scrollhelp.site/developer-docs/deployment-process) model, once code is in the `main` branch, it will be deployed within 24 hours. 

Before enabling your feature toggle in production, you'll need to:

- [ ] Follow [best practices for QA](https://depo-platform-documentation.scrollhelp.site/developer-docs/qa-and-accessibility-testing).
- [ ] Have your team perform as much validation in staging as possible. Validation may be challenging for some teams and systems due to downstream requirements, but the staging system should mimic the production system as much as possible.
- [ ] Work any downstream or dependant systems proactively to ensure that the feature is ready for use once it hits production.
- [ ] Have a go/no go meeting with the team to ensure that the feature is ready for use and signed off by each discipline and your DEPO/OCTO contact. During this meeting, you'll need to:
  - [ ] review the plan with your DEPO/OCTO representative.
  - [ ] review the release plan with your team.

## Step 3: Production rollout

### Do I need a staged rollout?

Yes

### Define the Rollback process

Our PM, Engineering Lead, Research Lead, and stakeholders will monitor analytics both on Google Analytics and DataDog. If they see a spike in errors or unexpected behavior, they will flag to the engineering team that there is a problem. The engineering team will do the following:
#### If a critical severity issue
1. The feature toggle will be disabled for all users
2. Debugging will start immediately
3. The fix will be tested and deployed through normal CI/CD practices
4. The feature toggle will be re-enabled, possibly at a lower percentage of logged-in users
#### If a high-severity issue
1. The feature toggle will remain enabled, possibly at a reduction of availability (e.g. lowered percentage of logged in users)
2. The offending commit will be reverted through normal `git` operations
3. Remove offending code by deploying the revert commit
4. A fix will be highly prioritized and worked on
5. The fix will be tested and deployed through normal CI/CD practices
#### If a low-severity issue
1. The toggle will remain enabled with no reduction in availability
2. A fix will be prioritized and worked on
3. The fix will be tested and deployed through normal CI/CD practices, with no interruption to feature uptime.

### Phase I: moderated production testing (also known as User Acceptance Testing, or UAT)

#### Planning

- Date range or test duration: Aug 22 - Sept 4
- Number of users: 2
- How you'll recruit the right production test users: We have identified two BAH Veterans
- How you'll conduct the testing: UAT
- How you'll give the test users access to the product in production w/o making it live on VA.gov: Through use of a feature flag

#### Results

- Number of users: 2
- Number of bugs identified / fixed: 2
  - STS Bug: Our service account wasn't configured correctly to include the ICN; team resolved this.
  - Incorrect config: The team was using out-of-date credentials; upstream service provided us with the updated credentials.
  - Overwrite issue: our request headers were getting overridden - the root cause was the need to support two API keys, but our team was able to resolve this as well.
- Was any downstream service affected by the change?: No
- Types of errors logged: N/A
- Any changes necessary based on the logs, feedback on user challenges, or VA challenges? [PICK_ONE]: No
- Latency: For succesful responses to the API, we had between 4 seconds and 23 seconds of latency. The team suspects that latency had a positive correlation with number of claims.

### Phase I: Friends and Family testing and 5% of users

#### Planning

- Desired date range or test duration: Week of Oct 15
- Desired number of users: 19
- How you'll recruit the right production test users: We have identified 19 Veterans with VA.gov email logins to whom we will reach out and offer to test our authenticated experience.
- How you'll conduct the testing: We will offer them an optimal workshop survey asking them to verify a few pieces of information.
- How you'll give the test users access to the product in production w/o making it live on VA.gov: Through use of a feature flag

#### Results

- Number of users: 5; we had users log in and view the page per GA, but no one provided direct feedback.
- Number of bugs identified / fixed: none
team was able to resolve this as well.
- Was any downstream service affected by the change?: No
- Types of errors logged: N/A
- Any changes necessary based on the logs, feedback on user challenges, or VA challenges? [PICK_ONE]: No
- Latency: For succesful responses to the API, we had between 4 seconds and 23 seconds of latency. The team suspects that latency had a positive correlation with number of claims.

### Phase II: Staged Rollout (also known as unmoderated production testing)

#### Rollout Planning

- Desired date range: January 13-24, 2025
- How will you make the product available in production while limiting the number of users who can find/access it: By using a feature flag.
- What metrics-based criteria will you look at before advancing rollout to the next stage ("success criteria")?: Because this entire page is an MVP without a specific entry point just yet, we will be broadly monitoring health and engineering metrics as the key indicator of whether or not we can advance.
- Links to the dashboard showing "success criteria" metrics: [Datadog Dashboard](https://vagov.ddog-gov.com/dashboard/crx-9dc-4y6/travel-pay-performance-dashboard?fromUser=false&refresh_mode=sliding&view=spans&from_ts=1723557083764&to_ts=1723643483764&live=true)
- Who is monitoring the dashboard(s)?: UX Leads, Engineering Lead, and Product Manager

**Engineering Metrics (Datadog)**
***[Datadog Dashboard](https://vagov.ddog-gov.com/dashboard/crx-9dc-4y6/travel-pay-performance-dashboard?fromUser=false&refresh_mode=sliding&view=spans&from_ts=1723557083764&to_ts=1723643483764&live=true)*** 

| Metric Name | Description |
| ----------- | ----------- |
| VA.gov - Endpoint error count | Number of non-200,201 status codes to va.gov endpoints |
| Travel Pay API - Endpoint error count | Number of non-200,201 status codes to travel pay API endpoints |
| VA.gov - Sustained high latency | p90 latency measures above x for time |

**User-Facing Metrics (Google Analytics and Call Center)**
***[Google Analytics Dashboard]([https://analytics.google.com/analytics/web/#/p419143770/reports/explorer?params=_u..nav%3Dmaui%26_r.explorerCard..filterTerm%3D%252Fmy-health%252Ftravel-claim-status%26_r.explorerCard..startRow%3D0&ruid=D4F7103F-DEA1-4A09-B066-EE554BF6F5F0&collectionId=8429185582&r=all-pages-and-screens](https://analytics.google.com/analytics/web/#/p419143770/reports/explorer?params=_u..nav%3Dmaui%26_r.explorerCard..filterTerm%3D%252Fmy-health%252Ftravel-pay%252Fclaims%26_r.explorerCard..startRow%3D0%26_u.comparisonOption%3Ddisabled%26_u.date00%3D20250112%26_u.date01%3D20250113&r=all-pages-and-screens&ruid=D4F7103F-DEA1-4A09-B066-EE554BF6F5F0&collectionId=8429185582))

| Metric Name | Description |
| ----------- | ----------- |
| Unique page views | # of individuals (% of users) who are visiting this page at least once |
| Repeat page views | # of times a given user is visiting this page |
| Are the users going out to BTSSS? | # of clicks on the help section's outbound link |
| Utilization of pagination | % of users going to the second page |
| Origin page | What page are they coming from to Travel Pay page? |
| Exit point | Where are they clicking out to? |
| Entry point | Which of the sources are they entering from (MHV / main page; VA Travel Reimbursement; My VA claims tool) |
| Time spent | Time spent on page |
| Call Center Queries | How many calls and queries did the VA call center receive about this page? |
| Member Services Queries | How many calls and queries did Member Services receive about this page? |


### Stage A: Canary

*Test a small Veteran population to ensure any obvious bugs/edge cases are found.*

#### Planning

- Length of time: 2 days, January 13 and January 14 
- Percentage of Users (and roughly how many users do you expect this to be): 5%

#### Results

- Number of unique users: 553
- Metrics at this stage (per your "success criteria"): please see detailed [MVP monitoring doc](https://github.com/department-of-veterans-affairs/va.gov-team/blob/master/products/health-care/beneficiary-travel/research/FeedbackSystems/RolloutMonitoring/MVPRolloutMonitoring0125.md)
- Was any downstream service affected by the change?: No
- Types of errors logged: 403, 400
- What changes (if any) are necessary based on the logs, feedback on user challenges, or VA challenges?: We decided we should implement clearer error messaging to explain when a user was getting an error related to access issues.

### Stage B: 25% of users

*Test a larger user population to ensure larger usage patterns expose no issues.*

#### Planning

- Length of time: 1 day, January 15
- Percentage of Users (and roughly how many users do you expect this to be): 25%

#### Results

- Number of unique users: 529
- Metrics at this stage (per your "success criteria"): please see detailed [MVP monitoring doc](https://github.com/department-of-veterans-affairs/va.gov-team/blob/master/products/health-care/beneficiary-travel/research/FeedbackSystems/RolloutMonitoring/MVPRolloutMonitoring0125.md)
- Was any downstream service affected by the change?: No
- Types of errors logged: 403, 400
- What changes (if any) are necessarily based on the logs, feedback on user challenges, or VA challenges? None, except for error messaging change

### Stage C: 50% of users

*Test a larger user population to ensure larger usage patterns expose no issues.*

#### Planning

- Length of time: 5 days, January 16 - 20
- Percentage of Users (and roughly how many users do you expect this to be): 50%

#### Results

- Number of unique users: 2,943
- Metrics at this stage (per your "success criteria"): please see detailed [MVP monitoring doc](https://github.com/department-of-veterans-affairs/va.gov-team/blob/master/products/health-care/beneficiary-travel/research/FeedbackSystems/RolloutMonitoring/MVPRolloutMonitoring0125.md)
- Was any downstream service affected by the change?: No
- Types of errors logged: 403, 400
- What changes (if any) are necessarily based on the logs, feedback on user challenges, or VA challenges?: None

### Stage D: 75% of users

*Test a larger user population to ensure larger usage patterns expose no issues.*

#### Planning

- Length of time: 2 days, January 21 - 22
- Percentage of Users (and roughly how many users do you expect this to be): 75%

#### Results

- Number of unique users: 2,933
- Metrics at this stage (per your "success criteria"): please see detailed [MVP monitoring doc](https://github.com/department-of-veterans-affairs/va.gov-team/blob/master/products/health-care/beneficiary-travel/research/FeedbackSystems/RolloutMonitoring/MVPRolloutMonitoring0125.md)
- Was any downstream service affected by the change?: No
- Types of errors logged: 403, 400
- What changes (if any) are necessarily based on the logs, feedback on user challenges, or VA challenges?: Error messaging for pages - work ongoing.

### Stage E: 100% of users

#### Planning

- Length of time: 1 day, January 23
- Percentage of Users: 100%

#### Results

- Number of unique users: 2,170
- Metrics at this stage (per your "success criteria"): please see detailed [MVP monitoring doc](https://github.com/department-of-veterans-affairs/va.gov-team/blob/master/products/health-care/beneficiary-travel/research/FeedbackSystems/RolloutMonitoring/MVPRolloutMonitoring0125.md)
- Was any downstream service affected by the change?: [PICK_ONE]: no
- Types of errors logged: 403, 400
- What changes (if any) are necessarily based on the logs, feedback on user challenges, or VA challenges? Error messaging deploys on Monday

## Post Launch metrics

Continue to check in on the KPIs of your feature at periodic intervals to ensure everything is working as expected. We recommend one-week and one-month check-ins, but this is only minimal.

### 1-week results

- Number of unique users: [FILL_IN]
- Post-launch KPI 1 actual: [FILL_IN]
- Post-launch KPI 2 actual: [FILL_IN]
- Post-launch KPI 3 actual: [FILL_IN]
- Any issues with VA handling/processing?:  [PICK_ONE]: yes | no |  N/A
- Types of errors logged: [FILL_IN]
- Any changes necessary based on the logs, feedback on user challenges, or VA challenges? [PICK_ONE]: yes | no |  N/A
- If yes, what: [FILL_IN]

### 1-month results

- Number of unique users: [FILL_IN]
- Post-launch KPI 1 actual: [FILL_IN]
- Post-launch KPI 2 actual: [FILL_IN]
- Post-launch KPI 3 actual: [FILL_IN]
- Any issues with VA handling/processing?: [PICK_ONE]: yes | no |  N/A
- Types of errors logged: [FILL_IN]
- Any UX changes necessary based on the logs, feedback on user challenges, or VA challenges? [PICK_ONE]: yes | no |  N/A
- If yes, what: [FILL_IN]

## Post-launch Questions

*To be completed once you have gathered your initial set of data, as outlined above.*

1. How do the KPIs you gathered compare to your pre-launch definition(s) of "success"?
1. What qualitative feedback have you gathered from users or other stakeholders?
1. Which assumptions you listed in your product outline were/were not validated?
1. How might your product evolve now or in the future based on these results?
1. What technical tasks are needed to clean up (i.e., removal of feature toggles)?
