# repochat

Ask questions about a GitHub repository without opening every file yourself. repochat is a Python repository chatbot that uses retrieval-augmented generation to pass relevant code and documents to a local language model, with a Streamlit chat interface.

## Features

- Fetch a GitHub repository into a local `cloned_repo` directory.
- Split repository files into chunks and embed them with `sentence-transformers/all-mpnet-base-v2`.
- Store embeddings locally in ChromaDB and retrieve relevant chunks for each question.
- Generate answers with a local GGUF model through `llama-cpp-python`.
- Retain conversation context during a chat session.

## Installation

You need Python, Git, and enough memory to load your chosen model. Native build tools may be needed to install `llama-cpp-python`.

Replace `YOUR_GITHUB_USER` below with the owner of this standalone repository. The URLs are templates, not published download locations.

```bash
git clone https://github.com/YOUR_GITHUB_USER/repochat.git
cd repochat
python -m venv repochat-env
source repochat-env/bin/activate
pip install -r requirements.txt
pip install llama-cpp-python
mkdir -p models
```

On Windows, activate the environment with `repochat-env\Scripts\activate` instead.

Download a compatible GGUF model, such as [CodeLlama 7B Q4_K_M](https://huggingface.co/TheBloke/CodeLlama-7B-GGUF/blob/main/codellama-7b.Q4_K_M.gguf), and place it in `models/`. In `models.py`, update the `model_path` argument inside `code_llama()` to point to that file:

```python
model_path="./models/codellama-7b.Q4_K_M.gguf",
```

Check the other `LlamaCpp` settings against your hardware. For optional GPU acceleration, follow the current [llama-cpp-python installation instructions](https://github.com/abetlen/llama-cpp-python); build flags vary by backend and version.

**Local inference still requires initial downloads.** Repository access, model weights, and embedding model files require network access unless already available locally.

Alternatively, once the standalone repository has a `v0.1.0` release, download its source archive using the tag associated with that release:

```bash
curl -L https://github.com/YOUR_GITHUB_USER/repochat/archive/refs/tags/v0.1.0.tar.gz -o repochat-v0.1.0.tar.gz
```

## Usage

Start the Streamlit application on port 8501:

```bash
streamlit run app.py --server.port 8501
```

Open the local URL printed in the terminal and enter a GitHub repository URL in the interface. repochat fetches the repository, splits its files, and builds a local ChromaDB index. Initial indexing takes time and depends on repository size and available hardware.

Once indexing finishes, ask a question such as:

```text
Where is the application entry point, and what does it initialize?
```

For each question, repochat retrieves relevant chunks and passes them to the language model with your input. Conversation memory provides context for follow-up questions.

**Check answers against the source.** Retrieved context can be incomplete, and the model can produce incorrect explanations.

## License

Licensed under Apache-2.0. This project is based on [pnkvalavala/repochat](https://github.com/pnkvalavala/repochat), which is also licensed under Apache-2.0. Retain applicable copyright, license, and attribution notices when redistributing the code.

## Related projects

- [bhautikvirani/naftis](https://github.com/bhautikvirani/naftis): Another repository in the supplied related-project list; consult its README for its scope.
- [MoveNSilence/nextjs-ollama-llm-ui](https://github.com/MoveNSilence/nextjs-ollama-llm-ui): A repository named for a Next.js UI for Ollama language models.
- [nocodemke-dev/minecraft-backup](https://github.com/nocodemke-dev/minecraft-backup): A repository named for Minecraft backups.

