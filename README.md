🌦️ MCP Weather Intelligence System

A chat assistant that answers weather questions using real-time data. A custom MCP (Model Context Protocol) server exposes a weather tool backed by the OpenWeatherMap API, and a CrewAI agent powered by Llama 3.3 (via Groq) decides when to call that tool.

Demo

<!-- Add a screenshot of a terminal chat in assets/demo.png -->
Features
Real-time weather for any city (description and temperature in °C)
Custom MCP server built with FastMCP that exposes a get_weather tool
CrewAI agent that uses the tool only when it is needed and answers other questions normally
Continuous terminal chat; type quit to exit
Simple error handling when a city is not found or the API fails
Tech Stack
Area	Tools
Language	Python
Agent framework	CrewAI
Tool protocol	Model Context Protocol (MCP), FastMCP server over stdio
LLM	Llama 3.3 70B (llama-3.3-70b-versatile) through the Groq API
Data source	OpenWeatherMap API
Config	python-dotenv
How It Works
You ──> CrewAI Agent (Groq LLM) ──> MCP client ──> weather.py (MCP server) ──> OpenWeatherMap API
                 ^                                                                     │
                 └─────────────────────────── weather result ─────────────────────────┘
You type a question, for example "What's the weather in Kochi and Delhi?"
The agent reads the question and decides whether it needs the weather tool.
If it does, it calls the MCP tool get_weather(city) on the MCP server.
The server fetches live data from OpenWeatherMap and returns a short result such as Kochi: Light rain, 27.4°C.
The agent turns the tool result into a friendly answer.
Getting Started
Prerequisites
Python 3.10 or newer
A free Groq API key
A free OpenWeatherMap API key
Installation
bash
git clone https://github.com/fida234/MCP-Weather-Intelligence-System.git
cd MCP-Weather-Intelligence-System
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
Add your keys

Create a .env file in the project folder (see .env.example):

GROQ_API_KEY=your_groq_key_here
OPENWEATHERMAP_API_KEY=your_openweathermap_key_here
OPM_WEATHER_URL=https://api.openweathermap.org/data/2.5/weather

Never commit your .env file.

Run
bash
python main.py
<!-- Change main.py to the real name of your chat/client file -->

The client starts the MCP server (weather.py) automatically. Then chat:

🤖 Chat with me! (type 'quit' to exit)

You: What's the weather in Kochi?
Bot: Kochi: Light rain, 27.4°C
Project Structure
MCP-Weather-Intelligence-System/
├── main.py             # CrewAI agent + chat loop (MCP client)  [CHECK FILE NAME]
├── weather.py          # MCP server with the get_weather tool
├── requirements.txt
├── .env.example
├── .gitignore
└── README.md
The MCP Tool
Tool	Input	Output
get_weather	city (string)	Location name, weather description and temperature in °C
Limitations
Returns current weather only (no forecast yet)
The tool handles one city per call; the agent calls it again for each extra city
Needs an internet connection and valid API keys
Future Improvements
Multi-day forecasts and weather alerts
Humidity, wind and "feels like" values
Web interface with Streamlit
More MCP tools (air quality, sunrise and sunset)
