# 🎬 YouTube Video Summarizer

A simple Streamlit web app that takes a YouTube link and gives you:

- a **5-line summary** of the video
- the **main topics with timestamps** (e.g. `[04:32] Topic name - short description`)

It reads the video's transcript and uses an LLM on Groq to summarize it.

## How it works

1. Extracts the video ID from the YouTube URL (supports `watch?v=`, `youtu.be/`, `shorts/` and `embed/` links).
2. Fetches the transcript with `youtube-transcript-api` (languages: English, Hindi, Marathi).
3. Splits the transcript into chunks of about 6000 characters, keeping each chunk's start time.
4. **Map step:** the LLM lists 2 to 4 main topics for each chunk.
5. **Reduce step:** the LLM combines all topic notes into the final 5-line summary and timestamped topic list.

Long videos are handled because every LLM call only sees one chunk at a time.

## Tech stack

- Python 3.9+
- [Streamlit](https://streamlit.io/) for the UI
- [youtube-transcript-api](https://pypi.org/project/youtube-transcript-api/) for transcripts
- [Groq](https://groq.com/) API with the `openai/gpt-oss-20b` model
- python-dotenv for loading the API key

## Setup

1. **Clone the repository**

   ```bash
   git clone https://github.com/YOUR-USERNAME/youtube-video-summarizer.git
   cd youtube-video-summarizer
   ```

2. **Install the requirements**

   ```bash
   pip install -r requirements.txt
   ```

3. **Get a free Groq API key** from [console.groq.com](https://console.groq.com/).

4. **Add your key** in one of two ways:

   - Create a file named `.env` in the project folder:

     ```
     GROQ_API_KEY=your_key_here
     ```

   - Or skip the file and paste the key into the app's sidebar when it runs.

## Run the app

```bash
streamlit run app.py
```

The app opens in your browser at `http://localhost:8501`. Paste a YouTube link and click **Summarize**.

## Project structure

```
youtube-video-summarizer/
├── app.py             # Streamlit app and summarization logic
├── requirements.txt   # Python dependencies
└── README.md
```

## Limitations

- The video must have captions or an auto-generated transcript. Videos with captions disabled will show an error.
- Only English, Hindi and Marathi transcripts are requested. Change the `LANGUAGES` list in `app.py` to add more.
- Summary quality depends on the transcript quality.

## Security note

Never commit your `.env` file or share your Groq API key publicly.
