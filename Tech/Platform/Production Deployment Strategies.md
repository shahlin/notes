A deployment strategy is any technique employed by DevOps teams to successfully launch a new version of the software solution they provide

# Types of Deployment Strategies
1. Blue/Green deployment (or Red/Black deployment)
2. Canary Deployment
3. Recreate Deployment
4. Ramped Deployment
5. Shadow Deployment
6. A/B Testing Deployment

## Blue/Green Deployment

> Blue/green deployments provide releases with near zero-downtime and rollback capabilities. The fundamental idea behind blue/green deployment is to shift traffic between two identical environments that are running different versions of your application. The blue environment represents the current application version serving production traffic. In parallel, the green environment is staged running a different version of your application. After the green environment is ready and tested, production traffic is redirected from blue to green. If any problems are identified, you can roll back by reverting traffic back to the blue environment.
> 
> *[From AWS](https://d1.awsstatic.com/whitepapers/AWS_Blue_Green_Deployments.pdf)*


- Two environments created - Blue and Green
	- Blue environment consists of existing updates
	- Green environment consists of newer updates
- Once Green env is tested, traffic is directed to it and the blue env is deprecated

![Illustration](https://storage.googleapis.com/cdn.thenewstack.io/media/2017/11/73a2824d-blue-green.gif)

### Advantages
- Isolation - Allows spinning up multiple Green environments without affecting the current stable environment (Blue)
- Reduced Downtime - Easy to rollback to stable version by just switching to Blue environment

### Disadvantages
- Cost - Can be expensive to replicate production environment
- Difficult when schema changes are complex to decouple OR data store is tightly coupled. It would require data synchronization between both environments

### Good when:
- We estimate the chance of failure is pretty low
- We're working in small safe iterations
- We need to switch all users at once

## Canary Deployment

Create replicas of the releases, direct some percentage of the traffic to the new replicas while still keeping maximum traffic on the existing instances (90:10% for example).
There's an agent checking for metrics of the new instances (error rate, response times, etc) - Basically validating if the new instance is ready for increased user reach (80:20% for example and increases slowly). Process continues until 100% of the users get newer version.

![Illustration](https://storage.googleapis.com/cdn.thenewstack.io/media/2017/11/a6324354-canary.gif)

### Advantages
- Allows testing with real users and use cases; Access to early feedback and bug identification
- Cheaper than Blue/Green because it doesn't require two different environments

### Disadvantages
- Frustration among the first group of people using the feature because they will find many bugs
- Same disadvantages as Blue/Green deployment

### Good when:
- We think we have a low-to-decent chance of failure
- We're concerned about performance or scaling issues
- We're doing a major update, like adding a shiny new or experimental feature
- We're OK with a slow rollout
- When testing is only possible on live production systems

## Recreate Deployment / Big Bang

Version A is terminated then version B is rolled out.

![Illustration](https://storage.googleapis.com/cdn.thenewstack.io/media/2017/11/c42fa239-recreate.gif)

### Advantages
- Cheaper
- Easy to setup
- Application state entirely renewed
- It doesn't require a load balancer as there's no shifting of traffic from one version to another in the live production environment

### Disadvantages
- Downtime between shutdown of Version A and startup of Version B

## Ramped Deployment / Rolling Updates

Gradually changes the older version to the new version. Default strategy for kubernetes deployments.

![Illustration](https://storage.googleapis.com/cdn.thenewstack.io/media/2017/11/5bddc931-ramped.gif)

### Advantages
- Zero downtime
- Allows quick deployments but also increases risks and complicates rollback process
- Easy to set up

### Disadvantages
- Rollout/rollback can take time - This is because the downgrading process to the initial version follows the same cycle, one instance at a time
- No control over traffic

## Shadow Deployment

A shadow deployment consists of releasing version B alongside version A, fork version A's incoming requests and send them to version B as well without impacting production traffic.

![Illustration](https://storage.googleapis.com/cdn.thenewstack.io/media/2017/11/fdd947f8-shadow.gif)

### Advantages
- Performance testing of the application with production traffic
- No impact on the user
- No rollout until the stability and performance of the application meet the requirements

### Disadvantages
- Expensive as it requires double the resources
- Complex to setup
- Requires mocking service for certain cases

## A/B Testing

A/B testing deployments releases the new version to a subset of users based on some criteria like location, operating system, language, etc. The primary goal of this strategy is to try out a feature for a subgroup and then roll out the features that yields the best results to all users.

![Illustration](https://storage.googleapis.com/cdn.thenewstack.io/media/2017/11/5deeea9c-a-b.gif)

### Advantages
- Several versions run in parallel
- Full control over the traffic distribution

### Disadvantages
- Complex to setup
- Requires intelligent load balancer
- Hard to troubleshoot errors for a given session, distributed tracing becomes mandatory

## Strategies Summary


# Database Rollback

## V1 Scenario

### Migrations
- Create Account table
- Create Transfer table

## V2 Scenario (Breaking change)

### Migrations
- Create Currency table (Master table for holding currencies like SAR, AED, etc)
- Add new non-nullable column to Transfer table - **CurrencyId**.

### Observation
- The above migration will fail if Transfer table has records already (existing records will have null values which is not allowed).

### Solution
- Release the breaking change in phases:
	- First add the new column as a nullable field
	- Gradually make sure all transfers have `CurrencyId` column populated
	- Make the `CurrencyId` column non-nullable
- In case there are unexpected failures during the migration, we can rollback using the undo scripts. Example:
	- **Flyway** - `flyway undo`
	- **Liquibase** - `mvn liquibase:rollback`

# Credits
1. Atef Najar
2. Riyanka Dagaonkar

# Bookmarks
- https://learn.microsoft.com/en-us/aspnet/web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-cs
- [Data Dog](https://www.datadoghq.com/)

# References
- https://d1.awsstatic.com/whitepapers/AWS_Blue_Green_Deployments.pdf
