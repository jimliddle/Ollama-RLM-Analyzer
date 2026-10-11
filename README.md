
Although this is not an RLM analyzer as per the recent MIT white paper, this implementation tries to honour the spirit of the RLM paper (https://arxiv.org/pdf/2512.24601v1) for a local Ollama model implementation

This implementation:

✅ Uses the model to analyze the objective and makes recommendations if it is too narrow or too broad

✅ Breaks the document into chunks on paragraph and sentence boundaries, with an overlap so that nothing is lost at a cut

✅ Each chunk gets its own LLM call with fresh context (in this case not to avoid context rot but because the context window in local LLM's is much smaller)

✅ Extraction Prompt returns structured facts, each with a supporting quote and a stated / inferred label (the extraction prompt is editable)

✅ Every quote is checked against the document in code, and each fact is marked verified, inferred or unmatched. Facts from a PDF cite their page

✅ Consolidation and audit run in batches sized to the context window, and audit corrections are merged back into the report

✅ Confidence rating at the end, with a count of the evidence behind the report and a warning for anything that was cut short

The REPL and code execution in the paper are implementation mechanisms, not the core insight which is  "Don't feed the whole thing to the model at once. Let it process pieces independently, then combine."

We are using a Fact Assertion pattern to ask the model:

1. Assert what it found
2. Quote the words in the document that support it
3. Distinguish between direct evidence and inference

The quote is then looked up in the source text by the app, so "verified" means the quote is really there. It does not mean the model's reading of the quote is right, which is why each fact is shown with its quote beside it.

This is a form of reasoning and verification which is not as dynamic as code execution, but it's there.

From the paper:

"Even without sub-calling capabilities, our ablation of the RLM is able to scale beyond the context limit of the model, and outperform the base model and other task-agnostic baselines on most long context settings."

This approach is analogous to this.

## Running it

All state is stored in the browser and the app will try and detect a running local ollama instance. The App can be run directly at: https://jimliddle.github.io/Ollama-RLM-Analyzer/

For the web page to detect Ollama and bypass CORS you need OLLAMA_ORIGINS to be set ie.

<code>
export OLLAMA_ORIGINS="*"
export OLLAMA_HOST="0.0.0.0:11434"   # optional; use if not only localhost
ollama serve
</code>

A few things to know:

- `*` lets any web page you visit talk to your Ollama. If you only use the hosted page, `OLLAMA_ORIGINS="https://jimliddle.github.io"` is enough.
- On a Mac the menu bar app does not read variables exported in a terminal. Quit Ollama, run `launchctl setenv OLLAMA_ORIGINS "*"` and start it again. This lasts until you log out.
- To run a local copy without setting anything, serve the folder with `python3 -m http.server` and open `http://localhost:8000`. Ollama accepts localhost origins by default. Opening `index.html` by double-clicking it does need `OLLAMA_ORIGINS="*"`.
- Structured extraction needs Ollama 0.5 or later.

### If the model list says "Failed to fetch"

The page has loaded, but either Ollama refused the request or the browser blocked it. Check in this order:

1. **Is Ollama running?** Open http://localhost:11434 in the browser. It should say "Ollama is running".
2. **Does Ollama allow the page?** Run this in a terminal:

   <code>curl -i -H "Origin: https://jimliddle.github.io" http://localhost:11434/api/tags</code>

   A 403 means `OLLAMA_ORIGINS` is not set for the copy of Ollama that is running. Set it and restart Ollama:
   - Mac: quit Ollama from the menu bar, run `launchctl setenv OLLAMA_ORIGINS "https://jimliddle.github.io"`, then start it again.
   - Windows: add `OLLAMA_ORIGINS` as a user environment variable, quit Ollama from the system tray, then start it again.
   - Linux with systemd: run `sudo systemctl edit ollama`, add `Environment="OLLAMA_ORIGINS=https://jimliddle.github.io"` under `[Service]`, then `sudo systemctl restart ollama`.
3. **Does the browser allow it?** If the curl command returns 200 and the page still fails, the browser is stopping a public web page from reaching localhost. Brave blocks this by default and recent versions of Chrome ask for permission to access the local network. Allow it for the site in the browser's site settings, or run a local copy as described above.

After any of these, click the refresh button next to the Endpoint URL in Ollama Settings.

## Settings

Under Ollama Settings:

- **Chunk Size** (default 6000 characters) and **Chunk Overlap** (default 500). The chunk size is reduced automatically if it would not fit the context window.
- **Context Window** (default 16384 tokens). This is sent to Ollama as `num_ctx` on every call. A larger window uses more memory.
- **Thinking** (model default, on, off) for reasoning models.

Reasoning models such as qwen3 spend output tokens on thinking before they answer. With a context window of 8192, qwen3.6:35b used more than 5,000 of roughly 5,800 available output tokens on thinking and its answers were cut off. At 16384 the same chunks finished. If you see a warning that a chunk "used all of its output room", raise the context window or turn thinking off.

The five prompts (refiner, extraction, consolidation, audit, merge) can be edited in the Prompt Lab.

## What to expect from a run

- Each chunk card shows the facts found, with a verified / inferred / unmatched badge, the page, and the quote.
- If a chunk fails or is cut short, the card and the final report say so, and the model's raw answer can be opened on the card.
- Extraction is saved chunk by chunk. If you cancel or hit an error, run again with the same document, objective and settings and it carries on from where it stopped.
- A long document can produce more facts than fit in one consolidation call. In that case the report is drafted in parts. The parts are merged if the result fits the context window, and shown as parts with a warning if it does not.
- The report, its evidence and any warnings can be exported as Markdown or PDF.

## Test document and results

The `test` folder has a fictional six page tender with 30 planted requirements, an answer key, and the report produced by qwen3.6:35b. That run found 30 of 30 requirements and 6 of 6 form fields. See [test/RESULTS.md](test/RESULTS.md) for the settings, the scoring and the limits of the test.

## Changes in October 2026

- Extraction returns JSON facts with quotes, and the quotes are checked against the document in code.
- Chunks break on paragraph and sentence boundaries with an overlap, and PDF page numbers are kept.
- `num_ctx` is set explicitly. Before, Ollama's default applied and long consolidation prompts were silently truncated.
- Consolidation and audit are batched to fit the context window. Audit corrections are merged into the report where they used to be appended to it.
- Temperature is 0 for every call.
- Streamed responses are buffered correctly, so text is no longer dropped when a line arrives in two pieces.
- Model output is rendered as sanitised Markdown. Markup in a document can no longer run as script in the page.
- Export from a saved analysis works when the text contains an apostrophe.
- Settings are remembered, and opening the settings dialog no longer resets the chosen model.
- Cancel button, saved progress and resume.
- Dropping a file onto the page attaches it.
- Clear History works.
- The page still works if one of its libraries fails to load, and script errors are shown in a bar at the top of the page.

<br/><br/>
The animated demo below was recorded with the earlier version. It shows an analysis of a sample tender downloaded from the web, using the Gemma3:7B model

![RLM_Explorer_ani_gif](https://github.com/user-attachments/assets/48d43704-1aec-48f6-a3e2-8fffa71981e6)
