# Portfolio Project Details

**Last Updated**: May 22, 2026

This repository will provide insights into my portfolio project. My project is a comprehensive data platform for Yahoo Fantasy Football analytics using AWS services including ECS Fargate, Lambda, Step Functions, RDS PostgreSQL, and S3. Due to the amount of personal time I have invested into my project, I am keeping the actual repository private. This public repository will go into details about the design of my project to highlight the key features &amp; learnings.

## Repository Overview: 
![alt text](portfolio_overview.png)

## Project Dataflow:
![alt text](project_dataflows.png)

## Example Output:
[Example_2025_week_17.pdf](example_2025_week_17.pdf)
* The included pdf is an example from my Fantasy Football Leagues Week 17 communication.

## Key Features

✅ **9 Data Extraction Pipelines** - ECS Fargate tasks extract data from Yahoo API  
✅ **Event-Driven Loading** - Lambda functions automatically load S3 data to RDS  
✅ **Step Functions Orchestration** - State machines manage complex workflows  
✅ **Multi-Layer Data Modeling** - Raw → Prep → Fact layer transformations  
✅ **Automated Data Quality** - Built-in validation and testing procedures  
✅ **Complete Observability** - CloudWatch monitoring, SNS alerts, execution tracking

## Key Components

| Component | Technology | Purpose |
|-----------|------------|---------|
| **Extraction** | ECS Fargate | Fetch data from Yahoo API |
| **Orchestration** | AWS Step Functions | Manage workflows, retries, monitoring |
| **Raw Storage** | S3 | Store JSON extracts, trigger processing |
| **Loading** | Lambda | Event-driven S3 → RDS transfer |
| **Data Warehouse** | RDS PostgreSQL | Multi-layer data model (raw/prep/fact) |
| **Transformations** | PostgreSQL Stored Procedures | Business logic, deduplication, aggregation |
| **Monitoring** | CloudWatch + SNS | Logs, metrics, alerts |
| **Secrets** | Secrets Manager | OAuth tokens, DB credentials |
| **Networking** | VPC + Endpoints | Secure, private communication |

## Design Decisions

| Decision | Details |
|----------|---------|
| **AWS Step Functions vs. Airflow** | Reduced costs (MWAA instance is much more expensive). Step Functions still provide visual execution graphs and detailed state tracking. |
| **Lambda vs. ECS for S3 to RDS Loading** | 50-75% cost reduction. Event-driven architecture eliminates scheduling. Faster cold starts than ECS tasks. |
| **Testing in Step Functions** | Integrated data quality testing within workflows. Alerts sent as SNS emails for consistent monitoring. |
| **Generic Runner File** | Single container image runs any object-specific job within the project. |
| **Idempotent Operations** | Uses SQL merging strategies (ON CONFLICT DO NOTHING & ON CONFLICT DO UPDATE) for S3 to Postgres loading. |

## Technology Stack

### AWS Services
- **ECS Fargate** - Containerized extraction jobs
- **AWS Step Functions** - Workflow orchestration (30+ state machines)
- **Lambda** - Event-driven processing and stored procedure execution
- **RDS PostgreSQL** - Multi-layer data warehouse (raw/prep/fact schemas)
- **S3** - Raw data lake storage
- **Secrets Manager** - OAuth tokens and database credentials
- **CloudWatch** - Logging, metrics, and alarms
- **SNS** - Email notifications for failures
- **EventBridge** - Scheduled job triggers
- **VPC** - Network isolation and security
- **ECR** - Container image registry

### Python Libraries
- **boto3** - AWS SDK
- **psycopg2** - PostgreSQL database driver
- **pandas** - Data manipulation
- **requests** - HTTP client for Yahoo API
- **yahoo-oauth** - Yahoo authentication

### Development Tools
- **PowerShell** - Deployment automation
- **Docker** - Container packaging
- **CloudFormation** - Infrastructure as Code
- **Amazon States Language (ASL)** - Step Functions definitions

## Evolution Timeline

### Phase 1: Basic Extraction (January - Early November 2025)
- Manual scripts for Yahoo API extraction
- Local development only
- Github link: https://github.com/95jhack/yahoo_fantasy_football_streaming
- Tech Stack: 
   - Local Postgres DB for data storage
   - Airflow DAGs for orchestration
   - Power BI for Dashboarding

### Phase 2: ECS + EventBridge (Early November 2025)
- Migrated to AWS ECS Fargate
- EventBridge scheduling for automation
- 2 objects: draft results, scoring settings
- S3 raw data storage

### Phase 3: Lambda Loading (Mid November 2025)
- Event-driven S3-to-RDS Lambda function
- Automated data loading pipeline
- Cost reduction vs. ECS loading approach
- 9 extraction objects operational

### Phase 4: Step Functions Orchestration (Late November 2025)
- Replaced EventBridge with Step Functions
- Visual workflow monitoring
- Built-in retry and error handling
- SNS alerting integration

### Phase 5: Stored Procedure Automation (December 2025)
- 13+ stored procedure state machines
- Configuration-driven deployment
- Multi-layer data modeling (raw → prep → fact)
- Automated data quality testing
- **Current state as of December 30, 2025**

## Contributing

This is a personal learning project demonstrating cloud data engineering best practices. Contributions are not currently accepted, but feedback is welcome.

## License

Internal project. Not for public distribution.

## Additional Resources

- [Yahoo Fantasy Sports API Documentation](https://developer.yahoo.com/fantasysports/guide/)
- [AWS Step Functions Documentation](https://docs.aws.amazon.com/step-functions/)
- [PostgreSQL Stored Procedures](https://www.postgresql.org/docs/current/sql-createprocedure.html)
