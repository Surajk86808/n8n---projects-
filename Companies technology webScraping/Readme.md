# 🤖 Automated Technology Stack Identification Workflow

This n8n workflow automates the process of identifying the technology stack for a list of company websites. It reads company domains from a Google Sheet, enriches the data by discovering the technologies used on their websites, and then writes this structured information back into the same sheet.

This is an incredibly powerful tool for market research, competitive analysis, or lead generation, transforming a simple list of domains into a rich dataset of technology usage.


---

## ✨ Key Features

* **Bulk Processing**: Reads a list of company domains directly from a Google Sheet.
* **Iterative Enhancement**: Loops through each company to fetch and process its data individually.
* **Advanced Web Scraping**: Utilizes **Jina AI** and **BuiltWith** to reliably fetch technology profile data for any website.
* **AI-Powered Data Structuring**: Employs an OpenAI model (`gpt-4o-mini`) to parse the raw data and intelligently categorize the technologies found.
* **Structured Output**: Formats the extracted technologies into clear categories like "Hosting & Servers," "CMS," "Analysis & Tracking Tools," etc.
* **Automated Data Entry**: Updates the Google Sheet with the newly found technology stack for each company, neatly organized in columns.

---

## 🚀 The Power of Jina AI & BuiltWith

Two key services make this workflow's data extraction both powerful and reliable:

* **BuiltWith**: This is the core data source. **BuiltWith is a web technology information profiler** that tells you what technologies a website is built with. The workflow queries BuiltWith's extensive database to get a raw profile of a company's tech stack.

* **Jina AI Reader**: Instead of accessing BuiltWith directly, we use the **Jina AI Reader (`r.jina.ai`) as an intelligent proxy**. It fetches the content from the BuiltWith page and returns a clean, structured, and easy-to-process version of the data. This strips away advertisements, boilerplate HTML, and other noise, providing the AI with only the essential information, which significantly improves the accuracy and reliability of the data parsing step.

---

## ⚙️ Workflow Breakdown

### 1. **Trigger and Read Data**

* **Node:** `When clicking ‘Test workflow’` & `Google Sheets`
* **Action:** The workflow starts manually. The first active node reads a list of companies from a specified Google Sheet, pulling data from the "Company Domain" column.

### 2. **Loop Over Each Company**

* **Node:** `Loop Over Items`
* **Action:** To process the list efficiently, this node ensures that each company domain is handled one at a time in the subsequent steps.

### 3. **Fetch Technology Data**

* **Node:** `Tech technologies sourcing` (HTTP Request)
* **Action:** For each domain, this node constructs a URL to query the **Jina AI Reader** with the company's **BuiltWith** profile (`https://r.jina.ai/builtwith.com/{{COMPANY_DOMAIN}}`). It sends the request with the necessary authorization and receives the clean, raw text data of the company's technology stack.

### 4. **Analyze and Structure with AI**

* **Node:** `Basic LLM Chain` + `OpenAI Chat Model` + `Structured Output Parser`
* **Action:** This is the AI-powered brain of the operation.
    1.  The raw text from the previous step is sent to the `gpt-4o-mini` model.
    2.  A specific prompt instructs the AI to analyze the text, identify all technologies, and organize them into predefined categories.
    3.  The `Structured Output Parser` forces the AI's response into a clean, predictable JSON format, ensuring the data is perfectly structured for the final step.

### 5. **Update Spreadsheet**

* **Node:** `Google Sheets1`
* **Action:** The final step. This node takes the structured JSON data from the AI.
* **Operation:** It uses the "Append or Update" function to find the correct row in the original Google Sheet (by matching the "Company Domain"). It then populates the corresponding columns ("Hosting & Servers," "CMS & Content Management," etc.) with the categorized lists of technologies.

---

## 🛠️ How to Set Up and Use

1.  **Prerequisites:**
    * An active n8n instance.
    * A Google account with a Google Sheet prepared.
    * An OpenAI API key.
    * A Jina AI API key.

2.  **Google Sheet Setup:**
    * Create a Google Sheet. The first row should be headers.
    * You must have a column named `Company Domain`.
    * Add columns for the technology categories you want to populate, e.g., `Hosting & Servers`, `CMS & Content Management`, `Analysis & Tracking Tools`, `Security & Performance`, and `Other Technologies`.

3.  **Configure Credentials:**
    * **Google Sheets:** Authenticate your Google account in n8n using OAuth2 and connect both `Google Sheets` nodes.
    * **OpenAI:** Add your OpenAI API key to your n8n credentials and connect the `OpenAI Chat Model` node.
    * **Jina AI:** The API key is used directly in the `HTTP Request` node's header. Replace the placeholder value with your actual Jina AI bearer token.

4.  **Run the Workflow:**
    * Populate the `Company Domain` column in your Google Sheet with the websites you want to analyze.
    * Run the workflow manually. Watch as it iterates through your list and fills in the technology data.

---

## 💡 Conclusion

This workflow is a powerful engine for automating technology-based market intelligence. By combining the strengths of n8n's automation, Google Sheets' data management, **BuiltWith's comprehensive data**, **Jina AI's intelligent parsing**, and OpenAI's analytical power, you can create valuable datasets with minimal manual effort.