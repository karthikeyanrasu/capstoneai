# SGC Conversational AI Chatbot
## Overview
The Shevet Glaubach Center (SGC) chatbot is designed to enhance career-related support for students, parents, and employers by automating responses to common inquiries. Built on the Retrieval-Augmented Generation (RAG) framework, powered by OpenAI’s GPT-3.5 Turbo, it provides accurate, contextually relevant guidance. This project integrates data from the SGC website and curated FAQs into a robust knowledge base, ensuring seamless and reliable user interaction.

## Features
- 24/7 Support: Accessible anytime, accommodating global users across time zones.
- Enhanced User Experience: Instant, accurate responses to FAQs about resumes, internships, job postings, and career events.
- Operational Efficiency: Reduces workload for SGC staff, enabling focus on high-value tasks.
- Scalable Deployment: Hosted on Azure for robust performance

## Use Cases
- Answering career-related FAQs.
- Providing guidance on internships, resume reviews, and job applications.
- Offering support during off-hours, weekends, and holidays.

## Technical Stack
- Framework: Retrieval-Augmented Generation (RAG).
- Backend: Flask.
- Embedding Storage: ChromaDB.
- Language Model: OpenAI GPT-3.5 Turbo.
- Tools and Libraries: LangChain, Flask, ChromaDB, Python.

## Project Setup
Prerequisites
- Python: 3.11 or later
- Visual Studio Code (VS Code)

### Installation
1. Clone the repository:
```bash
   git clone <repository_url>
   cd <repository_name>
```
2. Install dependencies:
```bash
   pip install -r requirements.txt
```
3. Environment variables:
   
   Web folder contains the .env file and also the folder Knowledge Base Creation also contains the .env file which contains the azure blob and openai api tokens.
4. Run the application locally:
   ```bash
   python app.py
    ```

   In case you run into issues comment out the first 3 lines of app.py . If u still face issues remove the pysqlite3-binary from requirements.txt and then run pip install again
5. Access the application:

   Open your browser and go to http://127.0.0.1:5000
## File Structure
```bash
   Web/
|-- app.py                 # Main application file
|-- requirements.txt       # Python dependencies
|-- .env                   # Environment variables
|-- templates/             # HTML templates
|-- static/                # Static files (CSS, JS, etc.)

```
## Deployment
### Deploy to Azure
1. Azure Setup:

    Use the existing App Service named SGCAIChatbot or create a new one if requirement comes.
2. Deployment Steps:
   - Install the Azure extension in VS Code.
   - Deploy the project to Azure using the "Deploy to Web App" option.
3. Post-Deployment:

    SSH into the server, extract deployment files if necessary, and restart the Web App service.
## How to Update the Knowledge Base

1. **Update the FAQ Document**:
   - Go to this link https://yuad.sharepoint.com/:x:/r/sites/TheShevetGlaubachCenterforCareerStrategyandProfessionalDevelopment-F24_DAVCapstoneProject/_layouts/15/Doc.aspx?sourcedoc=%7BC8C5A30E-5DD0-492F-BCCD-0C40A0935AA9%7D&file=SGC%20Chatbot%20Suggestions.xlsx&action=default&mobileredirect=true
   - Export as CSV into your local.
   - Navigate to Azure Blob Storage:  
     `aichatbotlogs -> Data Storage -> Container -> faqs`.  
   - Upload the latest version.  
     **Note:** Ensure the uploaded document has the same name as the original.
2. **Fork the Github repository**
   - In this repository Go to `Fork -> Create a new Fork `
3. **Clone this forked repository**
   - Go to the forked repository and click on `Code -> Copy the HTTPS link ` and then clone the respository in your local.
      ```bash
      git clone <https link>
     ```
4. **Set up the Python Version**
   - Go to https://www.python.org/downloads/release/python-3115/ and download the installer for your OS
   - Check if your python is installed in this directory `C:\Users\<username>\AppData\Local\Programs\Python\Python311`.**Make sure u replace your username**
   - For Windows type in `Edit the System Environment Variables -> Environment Variables -> Click on Path`
   - Add these two inside Path `C:\Users\<username>\AppData\Local\Programs\Python\Python311` and `C:\Users\<username>\AppData\Local\Programs\Python\Python311\Scripts` . **Make sure u replace your username**
  
5. **Activate the virtual environment**
   - Navigate into the `knowledge_base_creation` folder and run the command `<venv_name>\Scripts\activate`.**Make sure u replace the name with the actual env name**

6. **Run the Knowledge Base Script**:
   - Execute the script  `python create_knowledge_base.py`
     This script generates the vector space for embeddings created from web scraping and the FAQ document.  
     **Make sure**:
     - All dependencies are installed.
     - You run the script from the `knowledge_base_creation` folder.

7. **Generate and Replace the Vector Store**:
   - The script will generate a `vectorstore` file.
   - Copy the newly generated `vectorstore` file into the `web` folder, replacing the existing file.
8. **Make sure to commit and push your changes to the forked repository and also to the main repository so that the new vectorstore file is updated in the remote repository**

9. **Deploy the Web App**:
   - Redeploy the web app service to apply the updated knowledge base.

10. **Post-Deployment Actions**:
   - Navigate to **App Service** -> `SGCAIChatbot` -> **Development Tools** -> **Advanced Tools** -> **Bash**.
   - Go to the `wwwroot` folder.
   - Delete the existing vectorstore and extract the vectorstore from output.tar.gz
     ```bash
      rm -rf faq_scrape_vectorStore
      tar -xzvf output.tar.gz ./faq_scrape_vectorstore
     ```
11. **Restart the App Service**:
   - After completing the above steps, restart the app service to apply the changes.
