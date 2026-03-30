**Tags:** #backend #gemini #prompt-engineering
**File References:** `backend/extractor.py`, `backend/schema.py`

## The Anti-Hallucination Strategy
Initially, the LLM was "copy-pasting" example values (like "Spindle Shaft" or "EN8") directly from the Pydantic schema into the results. 
**The Fix:** We programmatically strip the `examples` key from the JSON schema dictionary *before* passing it to Gemini in `build_extraction_prompt()`.

## Multi-Page PDF Handling
Instead of a Map-Reduce loop, we leverage Gemini 2.5 Flash's massive multimodal context window.
* `pdf_to_images` converts all PDF pages to a list of PIL Images.
* The `contents` payload uses Python unpacking: `contents=[prompt, *images]` to send the entire document simultaneously.

## Data Validation Edge Cases
* **Nested Lists:** LLMs frequently fail at generating flat arrays of strings. We implemented a defensive `_sanitize_data` function that flattens `list[list[str]]` into `list[str]` for fields like `specific_finishes` and `special_instructions` before Pydantic validation.