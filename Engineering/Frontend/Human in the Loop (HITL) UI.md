**Tags:** #frontend #nextjs #state-management
**File References:** `frontend/src/components/app/ResultsViewer.tsx`, `frontend/src/lib/api.ts`

## The `editableResult` State
To build user trust, the AI's output is never final. The raw JSON from the backend is ingested into a local React state called `editableResult`.

* **Summary Metrics:** Mapped to unstyled HTML `<input>` fields.
* **Data Tables:** Custom `EditableTable<T>` components render standard `<td>` elements as editable inputs. 
* Any `onChange` event updates the `editableResult` state directly.

## Instant Export Pipeline
* **Old Way (Deprecated):** Sending the image back to `/extract/export` caused 120-second timeouts and duplicate LLM costs.
* **Current Way:** The `exportExcel` function in `api.ts` takes the `editableResult` JSON payload and POSTs it directly to the backend. Export latency is now < 1 second.

## Upcoming Features
* `[[Missing Data Highlighting]]: Need to implement logic to detect `null` values in critical fields and apply a red Tailwind border (`border-red-500`) to alert the user.