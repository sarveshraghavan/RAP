# RAP
# 🧠 AI News Summarizer (TinyLlama-powered)

An intelligent, real-time news summarization app built using the [TinyLlama 1.1B Chat Model](https://huggingface.co/TinyLlama/TinyLlama-1.1B-Chat-v1.0), capable of generating concise and clear summaries of the latest news articles in multiple tones (Formal, Casual, Bullet Points).

This tool is built using Gradio for the user interface and fetches news live using SerpAPI. It is designed to be run on **Google Colab with Python 3.10+ (as is currently standard on Colab).

---

## 🌟 Features

* 🔍 Real-time news fetching using SerpAPI
* ✂ Summarization using TinyLlama 1.1B Chat model
* 🎨 Multiple summary styles: Formal, Casual, Bullet Points
* 🖥 Clean and interactive Gradio interface
* 👋 Greeting detection for more natural interaction
* 💬 Easy topic extraction from casual user input
* 🧠 Designed for both beginners and technical users

---

## 🚀 Quick Setup (Google Colab)

> ⚠ This project is designed to run on Google Colab, not on local machines (unless compatible with dependencies).

### ✅ 1. Open in Colab

Click the button below to launch:

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

---

### ✅ 2. Install Required Libraries

Paste this into a Colab cell and run:


!pip install gradio transformers


---

### ✅ 3. Set Your SerpAPI Key

You’ll need a free or paid **[SerpAPI](https://serpapi.com/)** key to fetch live news.


SERP_API_KEY = "your-serpapi-key-here"  # Replace this in the code


---

### ✅ 4. Run the App

Once you've updated the API key, run the rest of the notebook. The Gradio interface will open inline or provide a link to open in a new tab.

---

## 🧑‍💻 How to Use

1. Enter your query, like:

   * Fetch me news on Google
   * Get me latest headlines about AI
   * News on climate change
   * Hello (for a greeting)

2. Choose a summary style:

   * Formal – concise and professional
   * Casual – conversational tone
   * Bullet Points – clear and to-the-point

3. Click "Fetch & Summarize".

4. The tool will:

   * Search news based on your topic
   * Generate summaries using the TinyLlama model
   * Display clickable article titles + AI-generated summaries

---

## 🔍 Example Output

Input Query: Fetch me news on Google  
Summary Style: Bullet Points

Output:

* Google announces AI improvements to Search.
* New Pixel devices expected to launch in September.
* Antitrust case filed against Google in EU.

---

## 🛠 Troubleshooting / FAQs

### ❓ Nothing shows up after clicking the button?

* Make sure you entered a valid query (e.g., "news on Microsoft").
* Ensure the SerpAPI key is active and added correctly.
* Try running the cell again in Colab.

### ❓ What is SerpAPI?

SerpAPI is a tool to fetch real-time search engine results via API. We use it to get the latest news articles.

### ❓ The summaries are long or cut-off?

We're using TinyLlama's text-generation pipeline with a token limit of max_new_tokens=150. You can tweak this in the code for longer or shorter summaries.

### ❓ Can I run this locally?

It's designed for Colab, but advanced users can run it locally with:


pip install gradio transformers


And by modifying the demo.launch() to demo.launch(server_name="0.0.0.0", share=True) for sharing.

### ❓ Is this free?

* TinyLlama is open-source.
* SerpAPI has a free plan (with limitations).

---

## ⚙ Technical Overview

### 🔢 Libraries Used

* transformers – for using Hugging Face models (TinyLlama)
* gradio – lightweight GUI
* requests – for API calls to SerpAPI
* re, os – general purpose utility
* typing – type hinting for clarity

### 🧠 Model Used

* Model Name: TinyLlama-1.1B-Chat-v1.0
* Provider: [HuggingFace Model Hub](https://huggingface.co/TinyLlama/TinyLlama-1.1B-Chat-v1.0)
* Task: text-generation
* Tokens per summary: ~150
* Sampling strategy: do_sample=True, temperature=0.7

---

## 🧱 Architecture Theory & Agent Design

### 🧠 What is an Agent in This Context?

In AI, an agent is a self-contained system that takes input, processes it with some intelligence, and produces an output. In this app:

* The user provides a query.
* The system (agent) breaks it into subtasks:

  1. Topic extraction
  2. Fetching news from SerpAPI
  3. Summarizing each result using a language model

This forms an autonomous summarization agent pipeline.

---

### 🧱 Architecture Breakdown

User Query ➝ [Input Handler]  
             ➝ [Topic Extractor]  
             ➝ [News Fetch Agent (SerpAPI)]  
             ➝ [Summarizer Agent (TinyLlama)]  
             ➝ [Gradio Output Renderer]

#### 🔄 Component Interaction:

1. Input Handler  
   Detects if the user just greeted ("hi", "hello", etc.) or provided an actual news query.

2. Topic Extractor  
   Cleans the query by removing keywords like “news on”, “get me”, etc. to isolate the topic.

3. Fetch Agent (Retriever)  
   Uses requests to call SerpAPI and fetch real-time news articles (top 3).

4. Summarizer Agent (Generator)
   * The TinyLlama model serves as the generator-based summarization agent.
   * It takes the article snippets and styles them using prompt engineering.
   * Depending on the tone (Formal, Casual, Bullet Points), the input prompt changes.

5. Output Renderer (Interface Layer)  
   Dynamically formats and displays results using Gradio's HTML output block.

---

### 🧠 Why Use a Generator Model?

Most modern summarization agents use encoder-decoder (seq2seq) architectures like T5 or BART.

However, TinyLlama is a decoder-only causal language model, which:

* Predicts the next token in a sequence (autoregressive)
* Responds well to prompting (in-context learning)
* Has been fine-tuned on chat-style and instruction-following tasks

That makes it ideal as a promptable summarization agent without needing fine-tuning.

---

### 🧰 Prompt Engineering Strategy

We guide the summarizer using natural language prompts based on the user-selected style.

#### Prompt Examples:

* Formal:
    
    Summarize this news in a professional tone:
    {article text}
    

* Casual:
    
    Hey! Can you explain this news casually?
    {article text}
    

* Bullet Points:
    
    Give 3 bullet point summary for:
    {article text}
    

These are passed directly into TinyLlama's text-generation pipeline, enabling flexible and stylistic summaries.

---

## 📜 License

This project is for educational and non-commercial use.

* TinyLlama is licensed under Apache 2.0.
* SerpAPI has its own [usage terms](https://serpapi.com/legal).
* Please abide by all respective licenses.

---

## 📧 Contact

For issues or contributions, feel free to fork or open an issue on GitHub.  
For questions related to the code, summarization techniques, or deployment, reach out via [your preferred contact channel].

---

## 🗂 Architecture Diagram


User Input
   |
   v
Company Name Extraction
   |
   v
+---------------------+
|   LangChain Agent   |
+---------+-----------+
          |
    +-----+-----+
    |           |
SerpAPI      HuggingFace
 News      (tinyllama1.1B)
Search      Summarizer
    |           |
    +-----+-----+
          |
   Formatted Output (Gradio)
