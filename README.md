
Although this is not an RLM analyzer as per the recent MIT white paper, this implementation tries to honour the spirit of the RLM paper (https://arxiv.org/pdf/2512.24601v1) for a local Ollama model implementation

This implementation:

✅ Uses the model to analyze the objective and makes recommendations if it is too narrow

✅ Break context into manageable pieces using chunking

✅ Each chunk gets its own LLM call with fresh context (in this case not to avoid context rot but because the context window in local LLM's is much smaller)

✅ Extraction Prompt with fact assertion + verifed / inferred behaviour (the extraction prompt is editable)

✅ Synthesis Prompt combines all extracted facts

✅ Confidence rating at the end

The REPL and code execution in the paper are implementation mechanisms, not the core insight which is  "Don't feed the whole thing to the model at once. Let it process pieces independently, then combine."

We are using a Fact Assertion pattern to aske the model:

1. Assert what it found
2. Qualify its confidence
3. Distinguish between direct evidence and inference

THis is a form of reasoning and verification which is not as dynamic as code execution, but it's there.

From the paper:

"Even without sub-calling capabilities, our ablation of the RLM is able to scale beyond the context limit of the model, and outperform the base model and other task-agnostic baselines on most long context settings."

This approach is analagous to this.

All state is stored in the browser and the app will try and detect a running local ollama instance. The App can be run directly at: https://jimliddle.github.io/Ollama-RLM-Analyzer/

For the web page to detect Ollama and bypass CORS you need OLLAM_ORIGINS to be set ie.

# Allow all origins and listen on all interfaces
export OLLAMA_ORIGINS="*"
export OLLAMA_HOST="0.0.0.0:11434"   # optional; use if not only localhost
ollama serve

The animated demo below shows an analyis of a sample tender downloaded from the web, using the Gemma3:7B model

![RLM_Explorer_ani_gif](https://github.com/user-attachments/assets/48d43704-1aec-48f6-a3e2-8fffa71981e6)


