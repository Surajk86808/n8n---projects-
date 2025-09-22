# 🤖 Automated AI-Powered Email Assistant Workflow

This n8n workflow demonstrates a powerful, automated system for processing incoming Gmail messages. It intelligently categorizes emails, applies appropriate labels, and sends personalized replies using the power of AI. It's a perfect example of how to build a smart, efficient, and scalable email management solution.



This project is designed to handle repetitive email tasks, freeing up valuable time while ensuring prompt and consistent communication.

---

## ✨ Key Features

* **Real-time Processing**: Polls your Gmail for new, unread emails **every minute**.
* **AI-Powered Information Extraction**: Uses an OpenAI model (`gpt-4.1-mini`) to intelligently extract the sender's name for personalization.
* **Dynamic Personalization**: Crafts personalized greetings. If a sender's name is found, it uses it ("Dear John Doe"); otherwise, it gracefully falls back to a generic greeting ("Hey,").
* **Intelligent Categorization**: Leverages AI to classify the content of the email into predefined categories (e.g., "course details," "education").
* **Automated Organization**: Automatically applies the correct Gmail labels based on the email's category.
* **Automated Replies**: Sends an immediate, templated reply acknowledging receipt, complete with the personalized greeting.

---

## ⚙️ Workflow Breakdown

This workflow follows a logical sequence of steps to process each email.

### 1. **Gmail Trigger**

* **Node:** `Gmail Trigger`
* **Action:** The workflow kicks off whenever a new **unread email** arrives in the connected Gmail account. It is configured to check for new mail every minute.

### 2. **Extract Sender Name**

* **Node:** `Information Extractor` + `OpenAI Chat Model`
* **Action:** The workflow takes the sender's information (`from.value[0].name`) from the trigger data.
* **AI Magic ✨:** It feeds this information to an OpenAI model, asking it to extract just the `SenderName`. This is a robust way to handle various formats of sender names (e.g., "John Doe <john.doe@example.com>" vs. "john.doe@example.com").

### 3. **Conditional Greeting Path**

* **Node:** `If`
* **Action:** This node checks if the `SenderName` was successfully extracted in the previous step. The workflow's path splits here based on the result.
    * **➡️ True Branch (Name Found):** If the `SenderName` is not empty, it proceeds to create a formal greeting.
    * **➡️ False Branch (Name Not Found):** If the name is empty, it creates a generic greeting.

### 4. **Set Personalized Introduction**

* **Nodes:** `Edit Fields` (True Path) & `Edit Fields1` (False Path)
* **Action:**
    * **True Path:** A new field called `Introduction` is created with the value `Dear {{SenderName}}`.
    * **False Path:** The `Introduction` field is set to `Hey,`.
* **Node:** `Merge`
* **Action:** The two separate branches are merged back into a single stream for the subsequent steps, ensuring that every execution has an `Introduction` field to work with.

### 5. **Prepare Data**

* **Node:** `Edit Fields2`
* **Action:** This node consolidates all the necessary data for the upcoming steps, setting variables for the `Introduction`, the `emailBody`, the `messageId`, and the `threadId` for easy reference.

### 6. **AI-Powered Email Classification**

* **Node:** `Text Classifier` + `OpenAI Chat Model1`
* **Action:** The core of the email-sorting logic. The text of the email body is passed to another OpenAI model.
* **AI Magic ✨:** The model is instructed to classify the email content into one of the predefined categories: **"course details"** or **"education"**. The output is a structured JSON object containing the classification.

### 7. **Apply Labels & Reply**

* **Nodes:** `Add label to message` & `Reply to a message`
* **Action:** Based on the classification from the previous step, the workflow executes the final actions in parallel branches.
    * If classified as **"course details"**:
        1.  A specific Gmail label is applied to the message.
        2.  A personalized reply is sent, using the `Introduction` variable and a standard acknowledgment text.
    * If classified as **"education"**:
        1.  A different, relevant Gmail label is applied.
        2.  The same personalized reply is sent.

---

## 🛠️ How to Set Up and Use

To get this workflow running in your own n8n instance, follow these steps:

1.  **Prerequisites:**
    * An active n8n instance.
    * A Gmail account.
    * An OpenAI API key.

2.  **Configure Credentials:**
    * **Gmail:** You'll need to create OAuth2 credentials for Gmail within n8n to allow the workflow to read and send emails on your behalf. Connect both `Gmail Trigger` and `Gmail` nodes to these credentials.
    * **OpenAI:** Add your OpenAI API key to your n8n credentials and connect both `OpenAI Chat Model` nodes.

3.  **Customize the Workflow:**
    * **Text Classifier:** Modify the categories in the `Text Classifier` node to match your specific needs (e.g., "Support Ticket," "Sales Inquiry," "Job Application").
    * **Gmail Labels:** In the `Add label to message` nodes, select the Gmail labels you want to apply for each category.
    * **Reply Message:** Edit the text in the `Reply to a message` nodes to customize the automated response.

---

## 💡 Conclusion

This workflow is a testament to the power of low-code automation combined with artificial intelligence. It serves as a practical, scalable solution for anyone looking to streamline their email management, improve response times, and maintain an organized inbox. It's not just a workflow; it's a blueprint for a smarter way to handle digital communication. 🚀