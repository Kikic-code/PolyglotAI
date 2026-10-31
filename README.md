# PolyglotAI – AI-Powered Multilingual Feedback Intelligence

PolyglotAI is an AI-powered multilingual feedback intelligence platform built using Zoho Creator and Google Gemini API. It helps businesses understand customer feedback by identifying sentiment, emotions, sarcasm, topics and urgency across multiple languages, including code-mixed text.
![PolyglotAI Dashboard](screenshots/dashboard.png)

## Features

- **Multilingual Feedback Collection:** Collect customer feedback across different languages.
- **Automatic Language Detection:** Identify the languages used in submitted feedback.
- **Sentiment Analysis:** Classify feedback as Positive, Neutral, Negative, Mixed or Uncertain.
- **Emotion Detection:** Identify emotions such as Joy, Anger, Sadness, Fear, Surprise, Disgust, Frustration and Trust.
- **Sarcasm Detection:** Identify potentially sarcastic expressions and contextual contradictions.
- **Topic Identification:** Extract the primary subject of customer feedback.
- **AI-Generated Summaries:** Generate concise summaries of customer experiences.
- **Urgency Classification:** Categorize feedback into Low, Medium, High and Critical priority levels.
- **Confidence-Based Review:** Flag uncertain AI predictions for human verification.
- **Automated Email Alerts:** Notify relevant users about feedback requiring attention.
- **Feedback Monitoring:** Track analysis results and review statuses through Zoho Creator reports.

## Technology Stack

| Component | Technology |
|---|---|
| Application Platform | Zoho Creator |
| Scripting Language | Deluge |
| AI Integration | Google Gemini API |
| Calendar Integration | Google Calendar API |
| Authentication | Google OAuth 2.0 |
| Data Management | Zoho Creator Forms and Reports |
| Automation | Zoho Creator Workflows |

## Application Architecture

The application follows an automated feedback processing workflow:

1. Customers submit feedback through the public feedback form.
2. Feedback is stored in Zoho Creator.
3. A workflow triggers the Gemini API integration.
4. Gemini analyzes the feedback and generates structured JSON output.
5. The analysis results are stored in the Feedback Analysis form.
6. Feedback with low confidence, mixed sentiment or possible sarcasm is flagged for human review.
7. High-priority feedback triggers automated email notifications.
8. Reviewers can verify and correct AI-generated predictions.

## Project Structure

```text
PolyglotAI/
│
├── PolyglotAI.ds
├── README.md
└── screenshots/
    ├── dashboard.png
    ├── feedback-form.png
    ├── analysis-report.png
    └── review-queue.png
```

The `.ds` file contains the exported Zoho Creator application structure, including forms, reports, pages, workflows and Deluge scripts.

## Installation and Setup

### 1. Prerequisites

- A Zoho Creator account.
- A Google AI Studio account.
- A Google Cloud account.
- A Gemini API key.
- Access to Google Calendar API if calendar integration is required.

### 2. Import the Application

1. Download `PolyglotAI.ds` from this repository.
2. Sign in to [Zoho Creator](https://creator.zoho.com/).
3. Navigate to the application import option.
4. Select the downloaded `.ds` file.
5. Complete the import process.
6. Open the imported PolyglotAI application.

The exported application contains its structure and configurations, not the original application's submitted feedback records.

### 3. Configure Gemini API Integration

PolyglotAI uses Google Gemini API (`gemini-3.5-flash-lite`) to perform multilingual feedback analysis.

> [!IMPORTANT]
> **Gemini API Key Placeholder:** For security reasons, actual API keys are not included in this repository. In the exported Deluge script, the Gemini API key is set to a placeholder:
> ```deluge
> apiKey = "your_gemini_model_api";
> ```
> You **must replace** `"your_gemini_api_key"` with your actual Gemini API key before running the application.

**Generate an API key:**

1. Visit [Google AI Studio – API Keys](https://aistudio.google.com/app/apikey).
2. Sign in with your Google account.
3. Generate a new Gemini API key.
4. Store the key securely.

**Update the API Key in Zoho Creator:**

1. Open the imported application in Zoho Creator **Edit** mode.
2. Navigate to **Workflows** → **Feedback_Submissions** → **Created** → **On Success** (Workflow: `Analyse_Feedback_Submissions`).
3. Click on the Deluge script action.
4. Locate the `apiKey` variable near the top of the script (line 677 of `PolyglotAI.ds`):
   ```deluge
   apiKey = "your_gemini_model_api";
   ```
5. Replace `"your_gemini_model_api"` with your actual Gemini API key.
6. Save the script.

**Security recommendation:** For production deployments, use a supported secure connection or secret-management mechanism instead of hardcoding API credentials in Deluge scripts.

### 4. Configure Automated Email Alert Recipient

PolyglotAI automatically sends email notifications when high-priority feedback, low-confidence predictions, negative sentiment, or possible sarcasm are detected.

> [!IMPORTANT]
> **Email Recipient Placeholder:** The recipient email in the exported Deluge script is set to a placeholder:
> ```deluge
> to :"polyglot@example.com"
> ```
> You **must change** `"polyglot@example.com"` to your actual administrator or notification email address to receive alert emails.

**Update the Recipient Email in Zoho Creator:**

1. In the same workflow script (**Workflows** → **Feedback_Submissions** → **Created** → **On Success** / `Analyse_Feedback_Submissions`), scroll down to the `sendmail` block (line 806 of `PolyglotAI.ds`):
   ```deluge
   sendmail
   [
       from : zoho.adminuserid
       to :"polyglot@example.com"
       subject : alertSubject
       message : alertMessage
   ]
   ```
2. Replace `"polyglot@example.com"` with your team or admin email address (e.g. `"admin@yourdomain.com"`).
3. Save the workflow script.

### 5. Configure Google Calendar Integration

PolyglotAI supports Google Calendar integration through OAuth 2.0 authentication using Zoho Creator Connections.

Each user must configure their own Google OAuth credentials.

**Step 1: Create a Google Cloud Project**

1. Visit [Google Cloud Console](https://console.cloud.google.com/).
2. Create a new project or select an existing project.
3. Navigate to APIs & Services → Library.
4. Search for Google Calendar API.
5. Enable the API.

**Step 2: Configure OAuth Consent Screen**

1. Navigate to Google Auth Platform.
2. Configure the OAuth consent screen.
3. Enter the application name and required contact information.
4. Configure the audience.
5. Add test users if the application is in Testing mode.
6. Configure the required Google Calendar scopes.

Example scopes:

- `https://www.googleapis.com/auth/calendar.events` – Manage calendar events.
- `https://www.googleapis.com/auth/calendar.readonly` – Read calendar information.

Use only the scopes required by the implementation.

**Step 3: Generate OAuth Credentials**

1. Navigate to Google Auth Platform → Clients.
2. Create a new OAuth client.
3. Select the appropriate application type supported by your Zoho connection.
4. Configure the authorized redirect URI using the callback URL provided by Zoho Creator.
5. Generate the OAuth Client ID and Client Secret.
6. Store these credentials securely.

**Step 4: Configure Zoho Creator Connection**

1. Open the imported PolyglotAI application.
2. Navigate to the Connections configuration area.
3. Create or configure the Google Calendar connection.
4. Enter your OAuth Client ID and Client Secret.
5. Configure the required scopes.
6. Authorize the connection using your Google account.
7. Save the connection.

Ensure that the connection link name matches the reference used in the Deluge scripts. Update the script references if you use a different link name.

**Step 5: Verify the Integration**

1. Open the calendar-related functionality.
2. Execute a calendar operation.
3. Complete the Google authorization process if prompted.
4. Verify that the operation executes successfully.

**Important:** Connection availability depends on the Zoho Creator subscription plan. Imported applications may require connections to be recreated and authorized. Google OAuth verification and consent requirements may also vary depending on the audience and scopes.

### 6. Test the Application

After completing the configurations:

1. Open the public Feedback Submission form.
2. Submit a sample customer review.
3. Allow the Gemini integration workflow to execute.
4. Open the Feedback Analysis report.
5. Verify the generated analysis.
6. Check whether uncertain feedback is added to the review queue.
7. Verify that high-priority feedback triggers the configured email alert.

### Expected Analysis Output

The AI-generated analysis includes:

| Attribute | Possible Output |
|---|---|
| Detected Languages | One or more detected languages |
| Sentiment | Positive, Neutral, Negative, Mixed, Uncertain |
| Primary Emotion | Joy, Anger, Sadness, Fear, Surprise, Disgust, Frustration, Trust, Other, Uncertain |
| Possible Sarcasm | Yes, No, Uncertain |
| Main Topic | Primary subject of feedback |
| Feedback Summary | Concise AI-generated summary |
| Urgency | Low, Medium, High, Critical |
| Confidence Level | 0–100 |
| Requires Review | Yes / No |

### 7. Human Review Workflow

Feedback requiring additional verification can be processed through the review management functionality.

Reviewers can:

- Verify AI-generated sentiment and emotion.
- Confirm or correct sarcasm detection.
- Add reviewer notes.
- Update review status.
- Record corrected predictions.

Review statuses include:

- Pending Review
- Reviewed
- Correction Required

## Security and Configuration Notes

- **Required Placeholder Changes:**
  - **Gemini API Key:** In `PolyglotAI.ds` (line 677), `apiKey = "your_gemini_api_key";` is a placeholder and **must be changed** to your personal API key from Google AI Studio.
  - **Alert Email Recipient:** In `PolyglotAI.ds` (line 809), `to :"polyglot@example.com"` is a placeholder and **must be changed** to your team or administrator email address.
- Never commit actual Gemini API keys to GitHub.
- Never publish OAuth Client Secrets, access tokens or refresh tokens.
- Use your own API credentials when configuring integrations.
- Review exported `.ds` files for sensitive information before publishing.
- Do not publish real customer feedback containing personal information.
- External connections may need to be recreated after importing the application.
- API usage is subject to Google's applicable quotas and pricing.
- Certain Zoho Creator features may depend on the subscription plan.

## Documentation and References

- [Zoho Creator](https://www.zoho.com/creator/)
- [Zoho Creator Deluge](https://www.zoho.com/deluge/)
- [Zoho Creator Connections](https://www.zoho.com/creator/help/application/connections.html)
- [Google AI Studio](https://aistudio.google.com/)
- [Gemini API Documentation](https://ai.google.dev/gemini-api/docs)
- [Google Calendar API](https://developers.google.com/workspace/calendar/api/guides/overview)
- [Google OAuth 2.0](https://developers.google.com/identity/protocols/oauth2)

## Disclaimer

PolyglotAI is an academic and development project demonstrating the application of AI-powered multilingual feedback analysis, workflow automation and third-party API integrations. Application records, real API credentials, and personal email addresses are not included. All API keys and email recipients in the Deluge scripts are placeholders and must be configured with your own values before running the application. Users importing the application must also ensure that their Zoho Creator subscription supports the required integrations.