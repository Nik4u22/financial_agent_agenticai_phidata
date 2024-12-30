# financial_agent_agenticai_phidata
Agentic AI app for financial stock recommendations using phidata platform

## What is the Agentic AI App?
Key use cases (e.g., intelligent agents for task automation, data analysis, customer support).
Technologies used (e.g., OpenAI GPT-4, Python etc.).
Target audience or industry.
Example:
The Agentic AI App is a cutting-edge application designed to create autonomous, task-driven AI agents capable of completing complex workflows with minimal user intervention. Powered by GPT-4 and a robust task management system, it enables seamless task delegation and intelligent decision-making.

## Installation
Step-by-step guide to set up the project locally:

### Prerequisites
Programming language runtime - python
Development Tool - visual studio code
Cloud/API services setup (e.g., OpenAI key, Phidata Api key, Groq Api Key).
Generate API Keys
  1. OpenAI API key - https://platform.openai.com/settings/organization/api-keys
  2. Phidata Api key - https://www.phidata.com/
  3. Groq Api Key - https://console.groq.com/keys

### 1. Set up Conda env - VSCode Terminal
conda create -p venv python==3.12

conda activate venv

### 2. Set API Keys - SETX Cmd
setx GROQ_API_KEY gsk_aDoqRSbs8DyVkL3jqcsgWGdyb3FYByyrufOT10pRi

setx OPENAI_API_KEY sk-proj-gat1or4_7vKeCE89Fiy93PwCY9pKZcAsFIWKoHY49dKpcyvjjB49SsQICRHgHRQNOWxyPw5oUFT3BlbkFJxe6Etl

### 3. Create requirements.txt file - add required libraries
phidata

python-dotenv

yfinance

packaging

duckduckgo-search

fastapi

uvicorn

groq

openai

### 4. Create .env file to handle API Keys
### 5. Create financial_agent.py file to create agents

import openai

import phi.api
from phi.model.openai import OpenAIChat
from phi.agent import Agent
from phi.model.groq import Groq

from phi.tools.yfinance import YFinanceTools
from phi.tools.duckduckgo import DuckDuckGo

import os
from dotenv import load_dotenv
load_dotenv()
openai.api_key=os.getenv("OPENAI_API_KEY")

### create web search agent 
web_search_agent=Agent(
    name="Web Search Agent",
    role="Search the web for the information",
    model=Groq(id="llama3-groq-70b-8192-tool-use-preview"), 
    tools=[DuckDuckGo()],
    instructions=["Always include sources"],
    show_tools_calls=True,
    markdown=True,
)

### create financial agent
finance_agent=Agent(
    name="Financial Agent",
    ##role="Search the web for the information",
    model=Groq(id="llama3-groq-70b-8192-tool-use-preview"),
    tools=[
        YFinanceTools(
            stock_price=True, 
            analyst_recommendations=True, 
            stock_fundamentals=True, 
            company_news=True,
        )
    ],
    instructions=["Use tables to display the data"],
    show_tools_calls=True,
    markdown=True,    
)

### create multi_ai_agent
multi_ai_agent=Agent(
    team=[web_search_agent, finance_agent],
    instructions=["Always include sources", "Use tabl to display the data"],
    show_tool_calls=True,
    markdown=True,
)

multi_ai_agent.print_response("Summarize analyst recommendation and share the latest news for NVDIA", stream=True)

### 6. Payground.py file - run your agent on phidata platform

![image](https://github.com/user-attachments/assets/f3506613-7537-4621-9510-bd6b9d885883)
