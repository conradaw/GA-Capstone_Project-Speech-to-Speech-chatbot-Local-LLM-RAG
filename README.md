<img src="http://imgur.com/1ZcRyrc.png" style="float: left; margin: 20px; height: 55px">

# DSI-SG-42
## Capstone Project: AI Voice Assistant for Bank customers
> Author: Conrad Aw
---

## Link to folder: https://drive.google.com/drive/folders/1AA2YTlJQeNp043Y4i-XfxrXDo1FU49Ku?usp=drive_link

### Requirements for this setup:
- ffmpeg installed (https://phoenixnap.com/kb/ffmpeg-windows)
- NVIDIA GPU (GPU utilization requires CUDA. However, CPU will work too)
- microphone
- local LLM setup (LM studio)
- Pytorch (https://pytorch.org/)

### Install (if you do not already have the files) and setup:

1. Set up virtual environment
2. pip install -r requirements.txt
3. download https://nordnet.blob.core.windows.net/bilde/checkpoints.zip
4. extract checkpoints.zip to speech-to-rag folder
5. In talk3.py (openvoice version):
- set your reference voice PATH on line 247
6. start LM studio server (or similar)
7. run talk3.py (low latency version)

## Contents
---
- [Executive Summary](#executive-summary)
- [Problem Statement](#problem-statement)
- [Data Collection](#data-collection)
- [Data Dictionary](#data-dictionary)
- [Data Cleaning, Preprocessing and EDA](#Data-Cleaning,-Preprocessing-and-EDA)
- [Modelling](#Modelling)
- [Conclusion](#Conclusion)
- [Recommendations](#Recommendations)

## Executive Summary
---
Suppose you're part of a AI/ML team at United Overseas Bank (UOB), responsible for the AI/ML stream for all projects that are implemented by UOB in Singapore. In recent discussions, Management has requested for a review of gaps that may currently exist in our digital solutions.

Traditional banking methods have often involved either physical visits to banks or navigating through complex online portals and mobile apps. These methods can be time-consuming and sometimes inaccessible for individuals with disabilities or those less technologically adept. Furthermore, the increasing customer expectation for instant and on-the-go services necessitates a shift towards more intuitive and responsive solutions.

The integration of AI in customer service, especially in the form of chatbots, has revolutionized how services are delivered. Speech recognition technology and sophisticated language models like LLaMA 3 allow chatbots to understand and process human language more effectively. These technologies can handle a wide range of customer interactions, from simple queries about account balances to more complex transactions and customer support services.

**Real-world problem**: Bank customers often encounter several obstacles that hinder their banking experience, including long wait times for customer service, limited access outside of regular business hours, and cumbersome navigation through automated systems. These issues are particularly acute for those less familiar with digital technology or with specific accessibility needs, contributing to frustration and dissatisfaction. In an era where personalized and efficient service is increasingly valued, there is a pressing need for banks to enhance how they interact with customers.

**Data science problem**: The proposed solution involves the development of an AI-powered voice assistant utilizing advanced technologies such as a Large Language Model (LLM) and Retrieval-Augmented Generation (RAG). This system will leverage the bank's frequently asked questions (FAQs) and other customer service resources to provide immediate, precise, and context-aware responses to customer inquiries. By integrating these technologies, the voice assistant aims to deliver a seamless and personalized customer service experience that aligns with United Overseas Bank’s (UOB) specific policies, products, and services.

## Problem Statement
---
The rapid digitization of banking has created accessibility challenges for elderly retirees with physical impairments, as traditional digital interfaces demand visual interaction and manual navigation. This highlights a critical need for an innovative solution that allows these individuals to manage their banking needs independently.

To bridge this gap, this project seeks to develop a speech-to-speech AI chatbot for United Overseas Bank (UOB), designed to answer FAQs and general queries. Utilizing Large Language Models for contextual understanding and Retrieval-Augmented Generation for accessing specific knowledge from the bank's FAQs, this chatbot is tailored to facilitate voice-only interaction, enabling users with physical limitations to engage with bank services informatively.

While it does not conduct transactions, the chatbot significantly enhances accessibility by providing reliable information and support, establishing a new standard in inclusive digital banking services.

## Data Collection
---
Source: https://www.uob.com.sg/personal/customer-service/general-help.page, https://www.uob.com.sg/personal/customer-service/payments-and-transactions.page#list-item-giro, https://www.uob.com.sg/personal/customer-service/credit-card.page, https://www.uob.com.sg/personal/customer-service/debit-card.page, https://www.uob.com.sg/personal/customer-service/pib-tmrw/index.page, https://www.uob.com.sg/personal/customer-service/tmrw-user-guide/index.page, https://www.uob.com.sg/personal/customer-service/atm.page, https://www.uob.com.sg/personal/customer-service/loans.page

We scraped the Questions and Answers content found in the above sources:

### 1. scraped_datav1.csv
- A clean dataset of Questions in column 1 and corresponding Answers in column 2.
- There are 291 pairs of these Questions & Answers (Q&A).
- Out of these 291 pairs, there are 266 unique questions and 261 unique answers.

### 2. qa_merged.txt
- This is a file that merged the Q&A pairs into the same row. e.g. "Questions: ..." "Answers:..."
- There is an empty row in between each Q&A pairs to mark them out distinctly.

### 3. vault.txt
- This is a file with appropriate URLs that are inserted to replace hyperlinks titled "Link" found in the qa_merged.txt file.

In our initial exploration, we used scraped_datav1.csv to evaluate the usability of the dataset for RAG. It was found to be appropriate and structure consistency was implemented and saved in qa_merged.txt. Thereafter, appropriate URLs were used to replace "Link" and saved as vault.txt.

vault.txt is the file that is used for RAG.

## Data Dictionary
---
### For raw data file 'scraped_datav1.csv':

|Feature|Type|Description|
|---|---|---|
|Questions|object|Customer Service Questions in UOB's customer service websites|
|Answers|object|corresponding Answers to Customer Service Questions in UOB's customer service websites|

## Data Cleaning and EDA
---
In our EDA, the following were found for the questions and answers (Q&A):
- Total number of Q&A pairs: 291
- Unique questions: 266
- Unique answers: 260
- Average word count in questions: 11
- Average word count in answers: 44

The columns in the raw dataset were merged to form Q&A pairs in the same row. e.g. "Questions: ..." "Answers:..."

There is an empty row in between each Q&A pairs to mark them out distinctly.

## Modelling Solution
---
Tools and Models used:
- Pytorch (Audio recording and playing)
- faster-whisper (Speech-to-Text)
- Llama3-8B (Large Language Model)
- RAG
- Openvoice (Text-to-speech)

## Conclusion
---
The speech-to-speech chatbot works well for English language and fulfills the minimum requirements.

## Recommendations
---
For future consideration, here are some ways in which the precision of a binary classification model could be improved.

1. **Multilingual Support**: To enhance the chatbot’s applicability in a multi-racial and multilingual country such as Singapore, adding support for multiple languages would be beneficial. This could involve integrating multilingual models for both speech recognition and synthesis.
2. **Performance Optimization**: Exploring options to reduce latency and increase response time through model optimization, such as using distilled versions of large models or deploying on more powerful servers, would improve user experience.
3. **Integration with system**: To provide more services
4. **Security Enhancements**: Given the use of personal data and interaction, ensuring robust security measures to protect user data and comply with privacy regulations is crucial.
5. **Expand Test Coverage**: Enhancing test scenarios to cover more real-world situations, different user inputs, and error conditions would help in making the system more robust.

By addressing these areas, the chatbot can be significantly improved and made ready for wider deployment in scenarios demanding high interactivity and user engagement.

---

### Files

**__pycache__**

**.venv**

**code**
- 01_Data_Collection_and_EDA.ipynb   
- 02_Modelling_&_Recommendations.ipynb (For explanation of code in talk3.py)
- 03_talk3.py (For running of chatbot)
- Demo (Powershell).txt (to run 03_talk3.py)
- api.py (supports chatbot file)
- attentions.py (supports chatbot file)
- chatbot3.txt (supports chatbot file)
- chatlog.json (supports chatbot file)
- commons.py (supports chatbot file)
- joa.mp3 (supports chatbot file)
- mel_processing.py (supports chatbot file)
- models.py (supports chatbot file)
- modules.py (supports chatbot file)
- openvoice_app.py (supports chatbot file)
- sameple.py (supports chatbot file)
- se_extractor.py (supports chatbot file)
- solutions_architecture_mapp.jpg (supports chatbot file)
- split.py (supports chatbot file)
- transforms.py (supports chatbot file)
- utils.py (supports chatbot file)

**dataset**
- scraped_datav1.csv (used for EDA)
- qa_merged.txt (not used)
- vault.txt (used for RAG)

**Slides**
- Capstone_Presentation_AI_Voice_Assistant_for_Banking.pdf

-.gitignore
- README.md
- requirements.txt
---