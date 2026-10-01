# Admission Application Processing (n8n Workflow)

An automated college admission workflow built with **n8n**. When a student submits the application form, the workflow checks eligibility, saves the application, and notifies the admissions team by email, with no manual work.

## Live Demo

| Item | Link |
|------|------|
| Live application form | [https://simransonaniya77.app.n8n.cloud/form/admission-application](https://simransonaniya77.app.n8n.cloud/form/admission-application) |
| Demo video | [Watch the demo on Google Drive](https://drive.google.com/file/d/1MxcODVdcdivorQom6-wBVQpilflw2_St/view?usp=sharing) |

> The live form runs on an n8n cloud trial account, which is valid for a limited time. If the form link has expired, please watch the demo video.

## Problem Statement

Without automation, an admissions officer must read every application, check the marks and age rules, type the details into a sheet, and email the team. This is slow and error-prone. This workflow does all of it automatically.

## Workflow Overview

```
Application Form -> Eligibility Rules -> Check Eligibility -> Save Application -> Notify Admissions Team
```

| # | Node | Type | What it does |
|---|------|------|--------------|
| 1 | Application Form | Form Trigger | Starts the workflow when a student submits the form and captures their details. |
| 2 | Eligibility Rules | Edit Fields (Set) | Stores the eligibility rules in one place (minimum 60% marks and age criteria) so they are easy to change. |
| 3 | Check Eligibility | Edit Fields (Set) | Compares each application with the rules and adds `eligibility_status` and `eligibility_reason`. |
| 4 | Save Application | Data Table | Saves every application and its result in the `admission_applications` table. |
| 5 | Notify Admissions Team | Gmail (Send message) | Emails the admissions team the applicant details and eligibility result. |

## Form Fields

- Full Name
- Email
- Phone
- Date of Birth
- Program (e.g. School, College - Undergraduate)
- Previous Marks Percentage

## Data Saved in the `admission_applications` Table

| Field | Description |
|-------|-------------|
| `applicant_name` | Student's full name |
| `email` | Student's email |
| `phone` | Student's phone number |
| `date_of_birth` | Date of birth |
| `age` | Calculated from date of birth |
| `program` | Program applied for |
| `previous_marks` | Previous marks percentage |
| `eligibility_status` | `Eligible` or `Not Eligible` |
| `eligibility_reason` | Why the student is eligible or not (e.g. "Marks 52% below minimum 60%") |
| `submitted_at` | Form submission timestamp |

## Eligibility Rules

- Minimum previous marks: **60%**
- Age criteria are checked against the program applied for

Rules live in the **Eligibility Rules** node, so they can be changed without editing the logic.

## Example Results

| Applicant | Marks | Result | Reason |
|-----------|-------|--------|--------|
| Riya Verma | 78% | Eligible | Meets all criteria |
| Rohan Mehta | 91% | Eligible | Meets all criteria |
| Aarav Sharma | 52% | Not Eligible | Marks 52% below minimum 60% |
| Karan Singh | 35% | Not Eligible | Marks 35% below minimum 60% |

## How to Import This Workflow

1. Download the `.json` file from this repository.
2. Open n8n and go to **Workflows**.
3. Click **Import from File** and select the downloaded `.json` file.
4. Open the **Notify Admissions Team** node, connect your own Gmail credential, and set the **To** email address.
5. Check the **Save Application** node points to a Data Table named `admission_applications` (create it with the fields above if needed).
6. Click **Execute workflow** to test, then **Publish** to make it live.

> Credentials are not included in the exported file for security reasons. You must connect your own Gmail account after importing.

## Tech Stack

- n8n (workflow automation)
- n8n Form Trigger
- n8n Data Table
- Gmail

## Possible Improvements

- Send a confirmation email to the student
- Use an IF node to send different emails for eligible and not-eligible applicants
- Validate inputs (email format, phone number, marks between 0 and 100)
- Add document upload (marksheet, ID proof)
- Prevent duplicate applications using email as a unique key

## Author

**Simran Sonaniya**
Email: simransonaniya77@gmail.com
GitHub: [https://github.com/Simransonaniya](https://github.com/Simransonaniya)
