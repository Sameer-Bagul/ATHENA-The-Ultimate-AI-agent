# ATHENA - AI Voice Assistant

A sophisticated AI voice assistant inspired by Iron Man's Friday, built with LiveKit and Google's Realtime AI model. This assistant provides real-time voice interaction with built-in tools for weather information, web search, and email functionality.

## Features

- **Real-time Voice Interaction**: Powered by Google's Realtime AI model with natural voice synthesis
- **Personality**: Classy butler persona with witty, sarcastic responses
- **Built-in Tools**:
  - Weather information for any city
  - Web search using DuckDuckGo
  - Email sending capabilities via Gmail
- **Noise Cancellation**: Enhanced audio quality with LiveKit's noise cancellation
- **Video Support**: Optional video functionality

## Prerequisites

- Python 3.8 or higher
- LiveKit Cloud account (or self-hosted LiveKit server)
- Google AI API access
- Gmail account with App Password (for email functionality)

## Installation

1. **Clone the repository**:
   ```bash
   git clone <your-repo-url>
   cd JARVIS
   ```

2. **Create a virtual environment** (recommended):
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

## Configuration

### Environment Variables

Create a `.env` file in the project root with the following variables:

```env
# LiveKit Configuration
WEBSOCKET_URL=wss://your-livekit-url.livekit.cloud
API_KEY=your_livekit_api_key
API_SECRET=your_livekit_api_secret
LIVEKIT_URL=wss://your-livekit-url.livekit.cloud
LIVEKIT_API_KEY=your_livekit_api_key
LIVEKIT_API_SECRET=your_livekit_api_secret

# Google AI API
GOOGLE_API_KEY=your_google_ai_api_key

# Gmail Configuration (Optional - for email functionality)
GMAIL_USER=your_gmail_address@gmail.com
GMAIL_APP_PASSWORD=your_gmail_app_password
```

### Required API Keys Setup

#### 1. LiveKit Setup
1. Sign up at [LiveKit Cloud](https://livekit.io/)
2. Create a new project
3. Get your API key, secret, and WebSocket URL from the project settings
4. Add these to your `.env` file

#### 2. Google AI API Setup
1. Go to [Google AI Studio](https://makersuite.google.com/app/apikey)
2. Create a new API key
3. Add the API key to your `.env` file as `GOOGLE_API_KEY`

#### 3. Gmail Setup (Optional)
1. Enable 2-factor authentication on your Gmail account
2. Generate an App Password:
   - Go to Google Account settings
   - Security → 2-Step Verification → App passwords
   - Generate a password for "Mail"
3. Add your Gmail address and app password to the `.env` file

## Usage

### Running the Assistant

1. **Start the agent**:
   ```bash
   python agent.py
   ```

2. **Connect to your LiveKit room** using the provided URL and access token

3. **Start talking** - The assistant will introduce itself as Friday and be ready to help!

### Available Commands

The assistant can help you with:

- **Weather queries**: "What's the weather in New York?"
- **Web searches**: "Search for the latest news about AI"
- **Send emails**: "Send an email to john@example.com with subject 'Meeting' and message 'Let's meet tomorrow'"

### Example Interactions

```
User: "Hi Friday, what's the weather like in London?"
Friday: "Of course sir, let me check that for you. London is currently 18°C with light rain."

User: "Can you search for Python tutorials?"
Friday: "Will do, Boss. I've found several excellent Python tutorials including beginner guides and advanced topics."

User: "Send an email to my colleague about the meeting"
Friday: "Roger Boss, I'll send that email right away. Email sent successfully to your colleague."
```

## Customization

### Modifying the Assistant's Personality

Edit the `prompts.py` file to customize Friday's responses:

```python
AGENT_INSTRUCTION = """
# Persona 
You are a personal Assistant called Friday similar to the AI from the movie Iron Man.

# Specifics
- Speak like a classy butler. 
- Be sarcastic when speaking to the person you are assisting. 
- Only answer in one sentence.
# Add your own personality traits here...
"""
```

### Adding New Tools

1. Create a new function in `tools.py`:
   ```python
   @function_tool()
   async def your_new_tool(
       context: RunContext,
       parameter: str) -> str:
       """
       Description of your tool.
       """
       # Your implementation here
       return "Result"
   ```

2. Import and add it to the Assistant class in `agent.py`:
   ```python
   from tools import get_weather, search_web, send_email, your_new_tool
   
   class Assistant(Agent):
       def __init__(self) -> None:
           super().__init__(
               # ...
               tools=[
                   get_weather,
                   search_web,
                   send_email,
                   your_new_tool  # Add your new tool here
               ],
           )
   ```

### Voice Configuration

Modify the voice settings in `agent.py`:

```python
llm=google.beta.realtime.RealtimeModel(
    voice="Aoede",  # Available voices: Aoede, Charon, Fenrir, Kore, Puck
    temperature=0.8,  # Adjust creativity (0.0 - 1.0)
),
```

## Project Structure

```
JARVIS/
├── agent.py           # Main agent implementation
├── prompts.py         # AI personality and instructions
├── tools.py           # Available tools (weather, search, email)
├── requirements.txt   # Python dependencies
├── .env              # Environment variables (create this)
├── .gitignore        # Git ignore file
├── readme.md         # This file
└── KMS/
    └── logs/         # Application logs
```

## Troubleshooting

### Common Issues

1. **Authentication Errors**:
   - Verify all API keys are correct in `.env`
   - Ensure Gmail App Password is generated and used (not regular password)

2. **Connection Issues**:
   - Check LiveKit URL and credentials
   - Ensure internet connection is stable
   - Verify firewall settings allow WebSocket connections

3. **Audio Issues**:
   - Check microphone permissions
   - Ensure proper audio drivers are installed
   - Try different browsers if using web interface

4. **Tool Errors**:
   - Weather: Check internet connection
   - Search: Verify DuckDuckGo is accessible
   - Email: Confirm Gmail credentials and App Password

### Logs

Check the `KMS/logs/` directory for detailed error logs and debugging information.

## Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature-name`
3. Make your changes and test thoroughly
4. Commit your changes: `git commit -m 'Add feature'`
5. Push to the branch: `git push origin feature-name`
6. Submit a pull request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- LiveKit for the real-time communication platform
- Google AI for the advanced language model
- Iron Man franchise for the inspiration
- Open source community for the various tools and libraries

## Support

If you encounter any issues or have questions:

1. Check the troubleshooting section above
2. Review the logs in `KMS/logs/`
3. Open an issue on GitHub with detailed information about the problem

---

**Enjoy your AI assistant! "At your service, sir." - Friday**