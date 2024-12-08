---
title: "Unlocking Google Gemini API in Colab for Free: A Practical Guide"
date: 2024-12-10
author: "Luciano Ayres"
---

If you’ve been dabbling in AI projects or just curious about leveraging powerful language models without breaking the bank, you’re in the right place. Today, we’re diving into how you can access Google’s Gemini API for free using a Google Colab notebook. 

While the free tier does come with some rate limits, it’s more than enough for study projects and experimentation. Let’s get started!

## Setting Up Secrets in Your Colab Notebook 🔒

First things first, you need a secure way to store your API key. Google Colab makes this easy with its **Secrets** feature.

1. **Locate the Secrets Tab:** On the left sidebar of your Colab notebook, look for the key icon 🔑. That’s your **Secrets** tab.
2. **Store Your API Key:** We’ll use this tab to keep your Gemini API key safe and out of sight from prying eyes or accidental leaks in your code.

## Generating Your Gemini API Key ✨🔑

To interact with the Gemini API, you need an API key. Here’s how to get one:

1. **Access the Secrets Tab:** Click on the **Secrets** tab (the key icon) in your Colab notebook.
2. **Manage API Keys:** Navigate to **Gemini API keys** and select **Manage Keys in AI Studio**.
   
   *Alternatively, you can go directly to:* [AI Studio API Keys](https://aistudio.google.com/app/apikey)

3. **Create Your Key:** Click on **Create API Key** and follow the prompts to generate your key.

## Testing Your API Key 🧑‍💻

Once you have your API key, it’s a good idea to test it to ensure everything is set up correctly. Google provides a cURL command for this purpose:

```bash
curl \
  -H 'Content-Type: application/json' \
  -d '{"contents":[{"parts":[{"text":"Explain how AI works"}]}]}' \
  -X POST 'https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash-latest:generateContent?key=YOUR_API_KEY'
```

Replace `YOUR_API_KEY` with the actual key you generated. Run this command in your terminal. If everything is set up properly, you should receive a response from the Gemini model explaining how AI works. This step ensures your key is valid and the API is accessible.

*Tip:* This cURL snippet is also available in the **Secrets** tab, making it easy to reference whenever needed.

## Integrating the API Key into Your Notebook 📝

Now that your API key is ready, let’s integrate it into your Colab notebook:

1. **Return to Secrets:** Go back to the **Secrets** tab in your notebook.
2. **Add a New Secret:** Click on **Add new secret**.
3. **Name Your Secret:** Enter `GOOGLE_API_KEY` as the name.
4. **Paste Your Key:** Insert your API key into the value field.
5. **Enable Access:** Ensure that **Notebook access** is enabled so your notebook can use the key.

In your first code cell, retrieve the API key with the following code:

```python
from google.colab import userdata
myKey = userdata.get('GOOGLE_API_KEY')
```

This stores your API key in the `myKey` variable. **Important:** Keep this key secure. Avoid printing or sharing the `myKey` variable to prevent unauthorized access.

**Note:** Deleting the secret from your notebook doesn’t invalidate the API key. To completely remove it, you need to delete it from AI Studio as well.

## Making Your First API Call 🚀

With your API key integrated, you’re ready to make your first call to the Gemini API. Here’s how:

1. **Add a New Code Cell:** In a new cell, paste the following Python code:

    ```python
    import requests

    url = "https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash-latest:generateContent"
    headers = {
        'Content-Type': 'application/json'
    }
    data = {
        "contents": [
            {
                "parts": [
                    {
                        "text": "Explain how AI works"
                    }
                ]
            }
        ]
    }

    response = requests.post(f"{url}?key={myKey}", headers=headers, json=data)
    print(response.text)
    ```

2. **Run the Cell:** Execute the cell. If everything is set up correctly, you should see a detailed response from the Gemini model explaining how AI works.

This simple call sends a prompt to the Gemini API and prints out the model’s response. It’s a great way to verify that everything is functioning as expected.

## BONUS ⭐
### Using the Google Gemini Python Library 

If you prefer a more streamlined approach, you can use the Google Gemini Python library to interact with the API. Here’s how:

1. **Install and Import the Library:**

    ```python
    import google.generativeai as genai
    from google.colab import userdata

    # Get your Google API key from userdata
    myKey = userdata.get('GOOGLE_API_KEY')
    genai.configure(api_key=myKey)
    ```

2. **Specify the Gemini Model and Define Your Prompt:**

    ```python
    # Specify the Gemini model
    model = genai.GenerativeModel("gemini-1.5-flash-latest")

    # Define the prompt
    prompt = "Explain how AI works"
    ```

3. **Generate and Print the Response:**

    ```python
    # Generate content using the Gemini model
    response = model.generate_content(prompt)

    # Print the response
    print(response.text)
    ```

Running these cells will yield a similar response to the previous method but leverages the convenience of the Google Gemini Python library, making your code cleaner and easier to manage.

## Where to Go from Here 🧭

Congratulations! You’ve successfully accessed the **free generative AI** capabilities of the Google Gemini API within a Colab notebook. Here are some next steps to continue your AI journey:

- **Experiment with Prompts:** Try different questions or tasks to see how the Gemini model responds. This can help you understand its strengths and limitations.
- **Explore Advanced Features:** Dive deeper into the Gemini API’s capabilities. Experiment with parameters and different endpoints to unlock more functionalities.
- **Build AI-Powered Projects:** Start integrating the Gemini API into your own projects. Whether it’s a chatbot, content generator, or data analyzer, the possibilities are vast.
- **Stay Updated:** AI is a rapidly evolving field. Keep an eye on the [Gemini API Docs](https://ai.google.dev/gemini-api/docs/quickstart?lang=python) for the latest features and best practices.
- **Try It Yourself:** Ready to get hands-on? [Open this Google Colab notebook](https://colab.research.google.com/drive/1dezbNUJUuSS55U-RRQu0OTPSdD7Bl1TT?usp=sharing) and follow along with the steps above to start experimenting with the Gemini API today!
