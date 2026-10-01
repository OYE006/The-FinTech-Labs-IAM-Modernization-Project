# The-FinTech-Labs-IAM-Modernization-Project

Scenario: FinTech Labs Inc.
You have just been hired as the Lead IAM Security Engineer at FinTech Labs, a fast-growing financial technology startup.
Up until now, the company used a traditional on-premise "Castle-and-Moat" network. However, following a recent near-miss security incident (where a developer's leaked password allowed an unauthorized script to touch customer records), executive leadership has mandated an immediate shift to an Identity-Centric, Zero Trust Security Model.
Your assignment is to complete a four-part IAM modernization proposal and audit.

Part 1: Identity Inventory & Taxonomy
FinTech Labs has a mix of human and non-human users. Categorize the following entities into the correct IAM taxonomy buckets:
1. Sarah: A software engineer who writes backend payment APIs.
2. Payment-Gateway-API-Key: An automated token used by the server to talk to Stripe.
3. Alex: A customer service representative who handles support tickets.
4. Lambda-Log-Processor: An AWS serverless function that scrapes audit logs every hour.

•	Your Task: Create a table listing each entity, identifying whether it is a Workforce Identity, Customer Identity, or Non-Human / Workload Identity, and defining its primary security risk if compromised.

Part 2: Designing a Least-Privilege Access Matrix (Authorization)
FinTech Labs has three primary sensitive resources:

•	Res-Dev-Code (Source code repository)

•	Res-Prod-Database (Customer financial records)

•	Res-IAM-Console (Cloud administrative panel)

The engineering team currently has full admin access to everything. You need to fix this using the Principle of Least Privilege (PoLP) and Separation of Duties (SoD).

•	Your Task: Design a clean access matrix using None, Read, or Read/Write for the following roles:

1. Software Engineer (Sarah)
2. Database Administrator (Bob)
3. DevOps Engineer (Dave)
   
•	Constraint: Ensure a developer cannot modify production databases, and a database administrator cannot push code updates (SoD).

Part 3: Incident Investigation & Audit Analysis (Accounting)
Last week, an unauthorized data read occurred on the production database at 2:00 AM. Management handed you an excerpt from the CloudTrail/Audit logs:


JSON
{

"timestamp": "2026-09-24T02:14:05Z",

"identity_type": "IAM Role",

"principal": "arn:aws:iam::123456789:role/DevOps-Deployment-Role",

"source_ip": "198.51.100.42",

"action": "rds:DownloadDBClusterSnapshot",

"status": "SUCCESS"

}

•	Your Task: Answer the following forensic questions using the AAA framework principles:

1. Identification & Authentication: Did a human directly log in, or was a workload identity used? Which account/role was invoked?
2. Authorization: Was the action permitted by default, or was there an explicit policy allowing it?
3. Accounting/Forensics: Based on the source IP and timestamp, what anomaly or red flag stands out that suggests a security incident? (Hint: Think about when developers usually deploy code and where traffic originates).
   
Part 4: The Zero Trust Transition Strategy
Write a brief executive summary (150–200 words) answering this question for the CEO:
•	"Why is our old network firewall (Castle-and-Moat approach) no longer enough to protect our cloud infrastructure, and how does shifting to Identity as the Perimeter solve our security gaps?"
Submission Checklist
- Part 1: Completed identity taxonomy table with risk analysis.
- Part 2: Built a least-privilege access matrix respecting Separation of Duties.
- Part 3: Answered forensic audit questions using the log snippet.
- Part 4: Drafted the Zero Trust executive summary.

      To design, audit, and secure an identity management framework for a growing tech company.
    
# Part 1: Completed identity taxonomy table with risk analysis.

<img width="787" height="328" alt="image" src="https://github.com/user-attachments/assets/d514c171-9792-41e5-8abe-7523e5c399b3" />


# Part 2: Built a least-privilege access matrix respecting Separation of Duties.

In this implementation, creating two cloud resources (simulating code repos and production databases using S3 buckets), writing custom least-privilege JSON policies to enforce Separation of Duties, create user groups, and test the permissions.

# Creating the Cloud Resources (S3 Buckets)

-	Bucket 1: Give it a name (e.g. fintech-dev-code-ao). Leave defaults and click Create bucket.

Navigate to Amazon S3 > Buckets > Create bucket

<img width="1362" height="507" alt="image" src="https://github.com/user-attachments/assets/4e0ddf97-ee75-48f9-b80b-a3a568ed75c8" />

-	Bucket 2: Give it a name (e.g. fintech-prod-data-ao). Leave defaults and click Create bucket.

Navigate to Amazon S3 > Buckets > Create bucket

<img width="1364" height="499" alt="image" src="https://github.com/user-attachments/assets/0f18a4eb-345b-4c8c-a6e4-d117294b9dc2" />

# Writing Custom Least-Privilege JSON Policies

    To enforce Separation of Duties (SoD) so that developers cannot touch production data and vice-versa

- Policy A: Software Engineer Policy

Navigate to IAM Dashboard > Policies > Create policy

Click the JSON tab and write the policy. The policy below grants Sarah access only to the development code bucket

<img width="1025" height="509" alt="image" src="https://github.com/user-attachments/assets/be8d9338-1028-4fab-908e-2bee82b34d4f" />

Click Next, name the policy (e.g. FinTech-SoftwareEngineer-Policy), and click Create policy.

- Policy B: Database Administrator Policy

Navigate to IAM Dashboard > Policies > Create policy

Click the JSON tab and write the policy. The policy below grants Bob access only to production data metrics/backups, completely blocking him from development code

<img width="1019" height="509" alt="image" src="https://github.com/user-attachments/assets/4332919c-d784-488d-9d82-2ac8e9fd23c6" />

Click Next, name it (e.g. FinTech-DBA-Policy), and click Create policy.

# Creating User Groups and Assign Personas
    Following best practices, assign these policies to Groups, not individual users.

Navigate to IAM dashboard > click User groups > Create group.

- Group 1: Name it SoftwareEngineers, Attach the (FinTech-SoftwareEngineer-Policy) policy. Click Create group.

<img width="1345" height="493" alt="image" src="https://github.com/user-attachments/assets/ef24dd15-cbc4-4c03-a5d6-958daf210030" />

- Group 2: Name it DatabaseAdmins, Attach the (FinTech-DBA-Policy) policy. Click Create group.

<img width="1360" height="500" alt="image" src="https://github.com/user-attachments/assets/97af429d-f053-4ed5-806e-9fc4ca6d75e4" />

Next, creating test users:

    Provide user access to the AWS Management Console and create passwords to log in and test)
    Note: Users must create a new password at next sign-in - Recommended
    
Navigate to IAM dashboard > IAM Users > Create user.

-	Creating a first user named Sarah-dev and adding her to the SoftwareEngineers group.

  <img width="1358" height="508" alt="image" src="https://github.com/user-attachments/assets/804e560a-e2a0-431e-812c-a71b4407acee" />

    User successfully created
<img width="1360" height="506" alt="image" src="https://github.com/user-attachments/assets/cd4bfa9f-d54b-4a93-8122-e69525220f6d" />

-	Creating a second user named Bob-dba and adding him to the DatabaseAdmins group.

<img width="1355" height="503" alt="image" src="https://github.com/user-attachments/assets/edbed76c-4fd8-49e5-be35-c4f179113df7" />

        User successfully created
<img width="1361" height="496" alt="image" src="https://github.com/user-attachments/assets/2b9df763-3488-4a0c-95cb-22d39c6586d7" />

# Test and Verify Separation of Duties
    Test as Sarah (sarah-dev):
-	Log into the AWS Console using Sarah's credentials.
-	Navigate to S3.
  
        Result: She can see both buckets listed, and when she clicks into fintech-dev-code-ao, she can read/write files.
 	
<img width="1364" height="591" alt="image" src="https://github.com/user-attachments/assets/0a7ae9fb-8e94-4887-86f0-b41ea988e0b5" />

<img width="1365" height="591" alt="image" src="https://github.com/user-attachments/assets/d91f29ba-bde3-4d95-8487-02ab79eeac08" />

    When she attempts to open fintech-prod-data-ao, AWS blocks her with an Access Denied message. Separation of duties enforced!

<img width="1360" height="600" alt="image" src="https://github.com/user-attachments/assets/d05b045d-6fea-402d-87a3-a4e3319ad253" />

    Test as Bob (bob-dba):
-	Log out and log back in as bob-dba.
-	Navigate to S3.
  
        Result: Bob can see both buckets listed; he can read and write inside the production data bucket.

 	<img width="1364" height="552" alt="image" src="https://github.com/user-attachments/assets/2e0f5318-36ad-4284-b083-d73326c18a81" />

  <img width="1358" height="566" alt="image" src="https://github.com/user-attachments/assets/bf82b953-edf8-4aff-ae97-75cee9eba49f" />

        When he tries to view or modify the development code bucket (fintech-dev-code-ao), he receives an Access Denied message.

  <img width="1357" height="540" alt="image" src="https://github.com/user-attachments/assets/5f8c54d6-978c-460c-ae43-5ef12f60c876" />


# Part 3: Answered forensic audit questions using the log snippet.

# Part 4: Drafted the Zero Trust executive summary.
