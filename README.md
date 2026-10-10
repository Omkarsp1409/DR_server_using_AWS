# DR_server_using_AWS
Built an AWS Disaster Recovery &amp; Business Continuity platform with cross-region EC2 recovery between Mumbai and Singapore, AMI backups, Amazon S3 Versioning, AWS Backup, VPC networking, and CloudWatch monitoring exploration, with a focus on recovery testing, technical documentation, security, and cost optimization.
# AWS Disaster Recovery & Business Continuity Platform

## Overview

A hands-on portfolio project demonstrating manual, cross-Region recovery
of a static Apache web application from Mumbai (`ap-south-1`) to
Singapore (`ap-southeast-1`), plus S3 object version recovery and an AWS
Backup EC2 image recovery point.

> **Scope note:** Automated failover (CloudWatch-triggered Lambda/Step
> Functions and Route 53 traffic switching) is a separate future project
> and is not implemented here.

## Architecture summary

  ----------------------------------------------------------------------------------
  Component               Mumbai primary            Singapore recovery
  ----------------------- ------------------------- --------------------------------
  Region                  `ap-south-1`              `ap-southeast-1`

  VPC                     `DR-Primary-VPC`          `DR-Singapore-VPC`

  CIDR                    `10.0.0.0/16`             `10.1.0.0/16`

  Application EC2         `DR-primary-app-AZ1`      `DR-Singapore-Recovery-App-01`

  AMI                     `DR-Primary-App-AMI-v1`   `DR-Recovery-App-AMI-v1`
  ----------------------------------------------------------------------------------

## Implemented and tested

-   Apache `httpd` installed and verified on the primary server
    (`active`, local HTTP 200 OK, browser access successful).
-   Primary AMI created and copied to Singapore; recovery AMI reached
    `Available`.
-   Recovery EC2 launched in Singapore and website verified in a
    browser; page updated to identify the DR environment.
-   S3 bucket created in Mumbai with versioning enabled; test of
    downloading an older object version completed.
-   AWS Backup on-demand EC2 image recovery point completed successfully
    with 7-day retention configured.
-   Both EC2 instances stopped between sessions to reduce compute costs.

## Monitoring status

CloudWatch EC2 metrics view was opened, but charts showed no data for
the selected time range. No CloudWatch alarms have been created.
Detailed monitoring and automatic failover are not configured.

## RTO and RPO

-   **Provisional RTO target:** within 60 minutes for a manual recovery
    of this static demo application.
-   **Measured RTO:** not yet recorded using a timed end-to-end
    exercise.
-   **RPO:** the recoverable point depends on the creation time of the
    latest available AMI or AWS Backup recovery point; S3 versioning
    permits restoration of retained previous object versions. A measured
    time-based RPO has not been established.
-   Targets are not guarantees or achieved SLAs.

## Commercial considerations

Potential charges include running EC2 compute, EBS storage, AMI
snapshots, AWS Backup storage, S3 storage/versions/requests, public
IPv4, and data transfer. Stopping EC2 does not eliminate all costs. No
NAT Gateway or load balancer was created in this phase.

## Documentation

-   `AWS_DR_Technical_and_Commercial_Documentation.docx` ---
    implementation summary, commercial considerations, evidence
    appendix.
-   `AWS_DR_Recovery_Runbook.docx` --- manual recovery steps,
    provisional RTO/RPO, troubleshooting, and test record template.
-   `screenshotforreference.docx` --- original project screenshot
    evidence supplied by the project owner.

## Future work

-   Perform a timed recovery exercise and record measured RTO/RPO.
-   Validate a restore from AWS Backup in a controlled window.
-   Review S3 lifecycle retention policy and billing.
-   Add monitoring only after choosing an appropriate cost-conscious
    approach.
