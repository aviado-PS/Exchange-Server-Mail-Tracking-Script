# Exchange Server Mail Tracking Script

## Description:

This script is designed to track and report on blocked email messages in an Exchange Server environment. It helps administrators identify emails that have been marked as 'FAIL' or 'DROP' by the Exchange server's filtering systems. These could be due to reasons such as spam detection, malware filtering, or specific transport rules.

The script is divided into three sections, each targeting a different time range:

1. Blocked mails from the last 24 hours.
2. Blocked mails from the last 2 days (48 hours).
3. Blocked mails from the last 7 days (168 hours).

Each section retrieves and displays logs with details such as timestamp, sender, recipients, subject, and the event type (FAIL or DROP) for the blocked emails. This information is crucial for analyzing mail flow issues, understanding the behavior of mail filters, and ensuring the reliability of email communication within the organization.

## Usage Instructions:

- This script must be run from an Exchange Management Shell on an Exchange Server.
- Ensure you have the necessary administrative privileges to execute these commands.
- You can run the entire script to get reports for all three time ranges or execute each section individually as needed.

**Note:** Modify the date ranges in the script if you need to track emails for different time periods.
## Author

This script was authored by [Aviad Ofek](https://github.com/aviado1).
## Begin Script

```powershell
# Exchange Server Mail Tracking Script

# Add PS Snapin for Exchange Management
Add-PSSnapin Microsoft.Exchange.Management.PowerShell.SnapIn

# SECTION: Retrieve Blocked Mails from Last 24 Hours
# Retrieves and displays logs for emails marked as 'FAIL' or 'DROP' within the past 24 hours.
$startDate24 = (Get-Date).AddHours(-24)
$endDate24 = Get-Date
Get-MessageTrackingLog -Start $startDate24 -End $endDate24 -ResultSize Unlimited | Where-Object { $_.EventId -eq "FAIL" -or $_.EventId -eq "DROP" } | Format-Table Timestamp, Sender, Recipients, MessageSubject, EventId

# SECTION: Retrieve Blocked Mails from Last 2 Days
# Retrieves and displays logs for emails marked as 'FAIL' or 'DROP' within the past 48 hours.
$startDate48 = (Get-Date).AddHours(-48)  # 2 days earlier
$endDate48 = Get-Date
Get-MessageTrackingLog -Start $startDate48 -End $endDate48 -ResultSize Unlimited | Where-Object { $_.EventId -eq "FAIL" -or $_.EventId -eq "DROP" } | Format-Table Timestamp, Sender, Recipients, MessageSubject, EventId

# SECTION: Retrieve Blocked Mails from Last 7 Days
# Retrieves and displays logs for emails marked as 'FAIL' or 'DROP' within the past 7 days.
$startDate168 = (Get-Date).AddHours(-168)  # 7 days earlier
$endDate168 = Get-Date
Get-MessageTrackingLog -Start $startDate168 -End $endDate168 -ResultSize Unlimited | Where-Object { $_.EventId -eq "FAIL" -or $_.EventId -eq "DROP" } | Format-Table Timestamp, Sender, Recipients, MessageSubject, EventId
