# Lightweight Modular Invoice Processing Automation Framework

![UiPath](https://img.shields.io/badge/Built%20With-UiPath-FA4616?style=for-the-badge&logo=uipath&logoColor=white)
![Automation](https://img.shields.io/badge/Domain-Invoice%20Automation-2b5d4b?style=for-the-badge)
![Architecture](https://img.shields.io/badge/Approach-Lightweight%20%7C%20Modular-456558?style=for-the-badge)

## Overview

This project is an end-to-end invoice processing automation built using UiPath. It is designed to automate a realistic finance workflow in which invoice emails are received through Gmail, PDF attachments are downloaded, invoice details are extracted and validated, duplicates are checked against an Excel-based register, valid records are posted into a desktop invoice entry application, notification emails are sent, and files are routed to processed or exception folders based on the final result.

The solution was intentionally developed using a lightweight and modular architecture. Instead of creating one monolithic workflow that handles every activity, the automation is split into dedicated components for inbox monitoring, attachment handling, data extraction, validation, duplicate checking, posting, and notifications. This structure reflects industry-standard RPA development practices by improving maintainability, readability, traceability, and reusability.

This repository is suitable for demonstrating:

- UiPath workflow orchestration
- Gmail automation using GSuite activities
- PDF text extraction
- Business-rule validation
- Excel-based duplicate control
- Desktop UI automation
- Exception routing and status notifications

## Key Features

- Automatically monitors a Gmail inbox for invoice-related emails with PDF attachments
- Downloads PDF attachments into a controlled local input folder
- Extracts invoice header values and line items from PDF files
- Validates mandatory fields, item row integrity, and amount consistency
- Detects duplicate invoices using an Excel register
- Posts valid invoices into a desktop invoice entry application
- Sends status-based notification emails after processing
- Moves files to processed or exception folders for operational traceability
- Uses a modular workflow-based design aligned with maintainable RPA project structure

## Why This Project Stands Out

This project follows a structured development approach that is close to industry-standard automation design. The workflows are separated by business responsibility, which makes the solution easier to debug and scale. The project also keeps the implementation lightweight by avoiding unnecessary infrastructure. It uses Gmail for intake, Excel for duplicate tracking, local folders for staging, PDF extraction for document understanding, and desktop UI automation for the final transaction step.

The result is a solution that is simple enough to be studied and demonstrated clearly, yet complete enough to represent a real-world automation pipeline.

## Process Flow

The diagram below shows the high-level invoice-processing pipeline implemented in this project.

<img width="1400" height="2020" alt="invoice-process-flowchart" src="https://github.com/user-attachments/assets/5d3f1d06-6323-4384-9f6f-a3546b682a77" />


## Project Structure

```text
Invoice Automation/
├── Main.xaml
├── project.json
├── project.uiproj
├── entry-points.json
├── Main.xaml.json
├── README.md
├── Invoice_Automation_Project_Report.md
├── Config/
│   └── Config.xlsx
├── Data/
│   ├── Input/
│   ├── Processed/
│   ├── Exception/
│   ├── Output/
│   │   ├── InvoiceRegister.xlsx
│   │   └── ProcessingLog.xlsx
│   ├── Logs/
│   └── Temp/
├── Workflows/
│   ├── Email/
│   │   ├── MonitorInbox.xaml
│   │   ├── DownloadAttachments.xaml
│   │   ├── MoveProcessedEmail.xaml
│   │   └── MoveExceptionEmail.xaml
│   ├── Extraction/
│   │   └── ExtractInvoiceData.xaml
│   ├── Validation/
│   │   ├── ValidateInvoice.xaml
│   │   └── CheckDuplicate.xaml
│   ├── Posting/
│   │   └── PostToSystem.xaml
│   └── Notifications/
│       └── SendNotification.xaml
└── report-assets/
    ├── invoice-process-flowchart.svg
    ├── invoice-process-flowchart.png
    ├── invoice-use-case-diagram.svg
    ├── invoice-use-case-diagram.png
    └── generate_diagram_pngs.ps1
```

## Workflow Breakdown

### `Main.xaml`

This is the main orchestrator of the project. It controls the complete invoice-processing lifecycle. It first monitors the inbox and downloads attachments, then loops through each PDF in the input folder and performs extraction, validation, duplicate checking, posting, notification, and final file routing.

### `Workflows/Email/MonitorInbox.xaml`

This workflow monitors Gmail for invoice-related emails. It filters the inbox using invoice-oriented search conditions and returns a list of matching emails for downstream processing.

### `Workflows/Email/DownloadAttachments.xaml`

This workflow receives the matched Gmail messages and downloads PDF attachments into the local input folder. It acts as the bridge between email intake and local file processing.

### `Workflows/Extraction/ExtractInvoiceData.xaml`

This workflow reads invoice PDFs and extracts structured business data from the document text. It captures:

- Company details
- Customer details
- Invoice number
- Invoice date
- Due date
- Subtotal
- GST
- Total
- Invoice line items

The extraction logic uses a combination of PDF text reading, text normalization, fixed-position mapping, and regex-based parsing.

### `Workflows/Validation/ValidateInvoice.xaml`

This workflow validates the extracted data before it is allowed to proceed. It checks:

- Presence of mandatory fields
- Availability of item rows
- Validity of item description, quantity, and price
- Mathematical consistency of subtotal, GST, and total

If an invoice fails business-rule validation, it is routed as a review or exception case instead of being posted.

### `Workflows/Validation/CheckDuplicate.xaml`

This workflow checks whether an invoice has already been processed. It reads `Data/Output/InvoiceRegister.xlsx` and compares the extracted company name and invoice number against the existing register entries.

### `Workflows/Posting/PostToSystem.xaml`

This workflow performs UI automation against the invoice entry desktop application. It fills company fields, customer fields, invoice metadata, item rows, and totals, submits the form, and confirms the success dialog.

### `Workflows/Notifications/SendNotification.xaml`

This workflow prepares and sends a status notification email after each invoice is processed. The message content changes depending on whether the invoice was processed successfully, marked duplicate, flagged for review, or failed.

### `Workflows/Email/MoveProcessedEmail.xaml`

This workflow currently exists as a placeholder for future enhancement and is not actively used in the current version.

### `Workflows/Email/MoveExceptionEmail.xaml`

This workflow also exists as a placeholder and is not currently wired into the active execution flow.

## Development Approach

### Lightweight Design

The project follows a lightweight approach by using practical and transparent components instead of heavy infrastructure. It avoids unnecessary complexity and uses:

- Gmail for email intake and notifications
- Local folders for input, output, and exception routing
- Excel for duplicate tracking
- PDF text extraction for reading invoices
- Desktop UI automation for system posting

This makes the solution easy to understand, easy to demonstrate, and easy to run in a controlled environment.

### Modular Design

The project follows a modular architecture where each workflow has a clear and focused responsibility. This approach improves:

- Maintainability
- Testability
- Readability
- Reusability
- Scalability

If a future change is required in one area, such as notification logic or duplicate checking, it can be updated with minimal impact on the rest of the solution.

### Industry-Standard Alignment

The solution reflects industry-standard RPA development principles such as:

- Separation of concerns
- Workflow reusability
- Clear process orchestration
- Structured exception handling
- Business-rule validation before transaction posting
- Operational traceability through file routing and status notifications

This makes the project not only technically functional, but also professionally structured.

## Setup Guide

Follow the steps below to run this project on your device.

### 1. Clone or Download the Repository

Clone the repository or download it as a ZIP file and extract it into a local folder.

### 2. Install Prerequisites

Make sure the following are available:

- UiPath Studio
- Microsoft Excel
- Gmail account access for email automation
- The desktop invoice entry application used by the project
- Follow the App-Setup.md to setup the invoice entry application for this project

### 3. Open the Project in UiPath Studio

Open the project folder in UiPath Studio. UiPath should detect the project using:

- `project.json`
- `project.uiproj`
- `Main.xaml`

### 4. Restore Project Dependencies

Once opened, allow UiPath Studio to restore the packages listed in `project.json`. The project depends on:

- UiPath System Activities
- UiPath Excel Activities
- UiPath GSuite Activities
- UiPath PDF Activities
- UiPath UIAutomation Activities

If any dependency is missing, install it through **Manage Packages**.

### 5. Preserve Folder Structure

Ensure the following folders remain available:

- `Data/Input`
- `Data/Processed`
- `Data/Exception`
- `Data/Output`
- `Data/Logs`
- `Data/Temp`
- `Workflows/Email`
- `Workflows/Extraction`
- `Workflows/Validation`
- `Workflows/Posting`
- `Workflows/Notifications`

The workflows rely on these locations for correct operation.

### 6. Configure Gmail Access

The Gmail activities use environment-specific connections. On another device, you will likely need to reconnect Gmail.

Recommended steps:

1. Open the Gmail-related workflows in UiPath Studio
2. Reconnect Gmail through UiPath Integration Service or GSuite activities
3. Re-authorize the account
4. Confirm that the Gmail activities point to a valid connection

### 7. Verify Output Workbook Availability

Check that the following file exists:

- `Data/Output/InvoiceRegister.xlsx`

This workbook is used for duplicate checking.

Also note:

- `ProcessingLog.xlsx` exists but is not actively used in the current workflow chain
- `Config/Config.xlsx` is present, but the current implementation does not actively consume its values

### 8. Update the Target Application Path

The posting workflow uses a local executable path for the invoice entry application. Open `Workflows/Posting/PostToSystem.xaml` and verify that the application target path matches the correct location on your machine.

If the executable path is different, update it before running the project.

### 9. Revalidate UI Targets if Needed

If your machine has a different screen resolution, application version, or interface structure, some selectors may need to be re-indicated.

Check:

- Company fields
- Customer fields
- Invoice fields
- Item grid
- Submit button
- Success dialog

### 10. Prepare Sample Invoices

To test the project:

- Send invoice emails with PDF attachments to the configured Gmail inbox
- Or place sample PDFs directly into `Data/Input` for downstream testing

### 11. Run `Main.xaml`

Start the project by running `Main.xaml` from UiPath Studio.

The expected process is:

1. Monitor Gmail inbox
2. Download invoice attachments
3. Extract invoice data
4. Validate extracted values
5. Check duplicates
6. Post to desktop application
7. Send notification
8. Move files to processed or exception folders

## Important Notes

- Gmail connections may need to be recreated on a new machine
- The desktop application path is environment-specific
- UI selectors may require revalidation depending on the device
- The extraction logic currently assumes a supported invoice format
- Two email workflows are placeholders and are not active in the current process

## Future Enhancements

This project can be extended further in several ways:

- Externalize configuration into `Config.xlsx`
- Write execution logs into `ProcessingLog.xlsx`
- Support multiple invoice templates
- Add OCR for scanned PDFs
- Automatically update the invoice register after successful posting
- Route processed and exception emails into dedicated Gmail folders
- Add attended review support for flagged invoices
- Improve resiliency for UI posting

## Conclusion

This project demonstrates a practical and professionally structured approach to invoice automation using UiPath. It combines multiple automation capabilities into a single end-to-end solution while maintaining a lightweight and modular architecture. The workflow separation, validation-first logic, duplicate control, and system posting design make it a strong example of how business automation should be built for clarity, maintainability, and future scalability.

It is not just a working automation project, but a well-organized framework that reflects strong development discipline and industry-aware design thinking.

## Contact

If you are reviewing this project for academic, hiring, or collaboration purposes, feel free to use the workflows, diagrams, and report structure in this repository as a reference for modular UiPath solution design.

You can contact me through mail if you have any questions on setup or other details, i will reply to your queries as soon as poosible

