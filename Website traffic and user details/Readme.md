# Automated B2B Cold Outreach Workflow (Slack → Email / LinkedIn)

This n8n workflow automates a sophisticated B2B cold outreach process. It triggers from a new Slack message containing lead information, enriches that data, uses AI to draft a personalized message, and then decides the best outreach channel—either a verified email or a LinkedIn campaign.

---

## How it Works

The workflow follows a logical path of data capture, enrichment, personalization, and intelligent distribution.

1.  **📥 Capture Lead from Slack**: The workflow starts when a message is posted in a specific Slack channel (`email-reply`). The **Slack Trigger** node captures this message.

2.  **🧹 Parse the Message**: A **Code** node uses regular expressions to parse the text of the Slack message, extracting key details like the lead's name, company, and LinkedIn profile URL.

3.  **✨ Enrich the Data**: The extracted information is sent to the **Dropcontact** node. Dropcontact finds and verifies professional contact information, enriching the lead with a valid email address, company size, and more.

4.  **🤖 Generate AI-Powered Copy**: The enriched data is passed to an **AI Agent** node, which uses an **OpenAI** model (`gpt-4o-mini`). It follows a detailed prompt to write a highly personalized, sub-50-word outreach email, signing off as "Manthan, Founder, Lead Gen Man".

5.  **⚙️ Format the AI Output**: A second **Code** node processes the AI's output, splitting it cleanly into separate `subject` and `body` fields for use in later steps.

6.  **🔀 Branch the Logic**: The workflow's primary **If** node checks if Dropcontact successfully found an email address for the lead.
    * If **YES**, the workflow proceeds down the email outreach path.
    * If **NO**, it pivots to the LinkedIn outreach path.

7.  **📧 Email Outreach Path**:
    * **Verify Email**: An **HTTP Request** node sends the found email to the Reoon Email Verifier API to ensure it's valid and safe to send to.
    * **Check Verification**: A second **If** node checks if the email status is "valid".
    * **Send Email**: If the email is valid, the **Gmail** node sends the personalized message. If not, the workflow redirects the lead to the LinkedIn path.

8.  **🔗 LinkedIn Outreach Path**:
    * **Add to Campaign**: An **HTTP Request** node connects to the HeyReach API and adds the lead to a specific cold outreach campaign (ID `115122`). The personalized AI message is included as part of the lead's data, likely to be used in a connection request or initial message.

---

## Nodes & Services Used

This workflow integrates the following services:

* **Slack**: To trigger the workflow and capture initial lead data.
* **Dropcontact**: For B2B data enrichment.
* **OpenAI**: For generating personalized email copy.
* **Reoon Email Verifier**: To validate email addresses before sending.
* **Gmail**: For sending verified cold emails.
* **HeyReach**: For managing and executing LinkedIn outreach campaigns.

---

## Setup & Configuration

To use this workflow, you will need to configure the following:

1.  **Credentials**: Set up valid credentials for all services used: Slack, Dropcontact, OpenAI, Gmail, and provide API keys for Reoon and HeyReach in their respective HTTP Request nodes.
2.  **Slack Trigger**: Update the `channelId` in the Slack Trigger node to match the channel you want to monitor in your own Slack workspace.
3.  **HeyReach Campaign**: In the "HTTP Request1" node, update the `campaignId` and `linkedInAccountId` in the JSON body to match your specific HeyReach setup.

---

## Expected Input Format

For the initial "Code1" node to parse the data correctly, the Slack message should follow this format:  
        Name: [Full Name]
        Company: [Company Name]
        Linkedin: [LinkedIn Profile URL]
        Email: [Email Address (if known)]
        Location: [Country or City, State]