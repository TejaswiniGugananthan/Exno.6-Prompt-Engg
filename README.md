# Exno.6-Prompt-Engg
# Date:
# Register no: 212222230157
# Aim: 
Development of Python Code Compatible with Multiple AI Tools



# Algorithm: 
Write and implement Python code that integrates with multiple AI tools to automate the task of interacting with APIs, comparing outputs, and generating actionable insights.
# Output:
```python
import os
import asyncio
import httpx
import openai
from dotenv import load_dotenv
import json

# Step 1: Load API keys
load_dotenv()
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")
COHERE_API_KEY = os.getenv("COHERE_API_KEY")
# Add more as needed

# Step 2: Define API Call Functions
async def call_openai(prompt):
    openai.api_key = OPENAI_API_KEY
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}]
    )
    return response.choices[0].message.content.strip()

async def call_cohere(prompt):
    headers = {"Authorization": f"Bearer {COHERE_API_KEY}"}
    json_data = {"prompt": prompt, "model": "command-r-plus", "max_tokens": 400}
    async with httpx.AsyncClient() as client:
        response = await client.post("https://api.cohere.ai/v1/generate", headers=headers, json=json_data)
        data = response.json()
        return data.get('generations')[0].get('text').strip()

# Step 3: Master function to call all models
async def query_all_models(prompt):
    tasks = [
        call_openai(prompt),
        call_cohere(prompt),
        # Add more calls for other models
    ]
    responses = await asyncio.gather(*tasks, return_exceptions=True)
    return responses

# Step 4: Comparison and Reporting
def generate_report(prompt, responses):
    report = {
        "Prompt": prompt,
        "Responses": [
            {"Model": "OpenAI GPT-4", "Response": responses[0]},
            {"Model": "Cohere Command-R+", "Response": responses[1]},
            # Extend for others
        ]
    }
    return report

# Step 5: Main Execution
if __name__ == "__main__":
    user_prompt = input("Enter your prompt: ")
    responses = asyncio.run(query_all_models(user_prompt))
    report = generate_report(user_prompt, responses)
    
    with open("ai_comparison_report.json", "w") as f:
        json.dump(report, f, indent=4)
    
    print("Report generated: ai_comparison_report.json")

```




# Result: 
The corresponding Prompt is executed successfully
