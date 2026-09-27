# ComicCraft — AI Comic Story Creator

ComicCraft is a complete FastAPI web application that turns a story prompt into a personalized AI-generated 5-panel comic strip (narrative, dialogue, and comic panels), previewable in the browser and exportable as a multi-page PDF comic book.

---

## Tech Stack
- **Backend:** FastAPI + Uvicorn
- **Templating:** Jinja2
- **Text Generation:** Google Gemini API — `gemini-1.5-flash` (5-panel outline) + `gemini-1.5-pro` (full narration/dialogue script)
- **Image Generation:** Hugging Face Diffusers — Stable Diffusion (`runwayml/stable-diffusion-v1-5`) with Hugging Face Inference API and stylized comic fallback support
- **PDF Export:** FPDF (`fpdf2`)
- **Environment Management:** python-dotenv
- **Form Handling & Validation:** python-multipart, Pydantic

---

## Project Structure
```
d:/Comiccraft/
  ├── app/
  │   ├── __init__.py
  │   ├── main.py              # FastAPI app initialization, static mount, route inclusion
  │   ├── routes.py            # Endpoints: /, /generate, /generate-comic/json, /export-success, /test-image
  │   ├── gemini_flash.py      # generate_outline() using gemini-1.5-flash
  │   ├── gemini_pro.py        # generate_story() using gemini-1.5-pro
  │   ├── image_generator.py   # generate_image() using Stable Diffusion / HF API
  │   ├── layout_builder.py    # build_comic_layout() merging outline, story, and images
  │   ├── exporters.py         # save_pdf() compiling multi-page comic PDF
  │   ├── templates/
  │   │   ├── index.html           # Homepage form with 5 fields & quick presets
  │   │   ├── comic_preview.html   # 5-panel comic strip preview & PDF download
  │   │   └── export_success.html  # Export confirmation page
  │   └── static/
  │       ├── style.css        # Vibrant comic book theme & responsive layout
  │       ├── script.js        # Form validation, preset population, loading step modal
  │       ├── panels/          # Saved comic panel images
  │       └── exports/         # Saved PDF comic strips
  ├── .env                     # API keys configuration
  └── requirements.txt         # Project dependencies
```

---

## Setup & Running

### 1. Activate Environment & Install Dependencies
```bash
# Windows
comiccraft-env\Scripts\activate

# macOS / Linux
source comiccraft-env/bin/activate

pip install -r requirements.txt
```

### 2. Configure Environment Variables
Edit `.env`:
```env
GEMINI_API_KEY=your_gemini_api_key_here
HF_API_KEY=your_huggingface_api_key_here
```
*(If keys are not yet configured, the app includes fallback generators so you can preview the full end-to-end comic generation pipeline immediately!)*

### 3. Run the Web Application
```bash
uvicorn app.main:app --reload
```
- Web Application: [http://127.0.0.1:8000](http://127.0.0.1:8000)
- Interactive API Docs (Swagger): [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)

---

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/` | Renders homepage form |
| `POST` | `/generate` | Reads form fields (`story_prompt`, `character_name`, `setting`, `story_tone`, `art_style`) and renders `comic_preview.html` |
| `POST` | `/generate-comic/json` | JSON endpoint accepting `PromptRequest` and returning comic layout + PDF file path |
| `GET` | `/export-success` | Renders PDF export confirmation screen |
| `POST` | `/test-image` | Directly invokes `generate_image()` with custom prompt for quick image testing |
"# ComicCraft_12" 
"# ComicCraft_12" 
