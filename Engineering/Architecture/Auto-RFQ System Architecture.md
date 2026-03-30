**Tags:** #architecture #overview 
**Status:** Active (MVP)

## Tech Stack
* **Frontend:** Next.js (React), Tailwind CSS, TypeScript
* **Backend:** FastAPI (Python 3.11), Pydantic v2
* **AI Model:** Gemini 2.5 Flash (via `google-genai` SDK)

## Core Data Flow
1. **Upload:** User drops an engineering drawing (PDF, JPG, PNG) into the Next.js `FileDropzone`.
2. **Inference (`POST /extract`):** * FastAPI converts PDFs to PIL Images (at 200 DPI).
   * All pages are unpacked into a single Gemini 2.5 Flash context window.
   * Gemini extracts data into a JSON structure defined by our `RFQExtraction` Pydantic schema.
3. **Sanitization:** Backend runs `_sanitize_data()` to flatten any nested arrays returned by the AI, preventing `ResponseValidationError`.
4. **HITL Editing:** Next.js receives the JSON and maps it to `editableResult` state in `ResultsViewer.tsx`. The user can manually correct hallucinations or missing data.
5. **Export (`POST /extract/export`):** User clicks export. Frontend sends the *edited JSON body* (not the image) back to FastAPI, which instantly generates an `openpyxl` Excel file and returns it.

## Known Limitations
* Hard limit on 6GB VRAM for local inference (currently bypassed by using Gemini API).
* Very large A0 blueprints might lose resolution when compressed into Gemini's context window.