# ShellPromptor

ShellPromptor is a tool designed to prime the context of Large Language Models (LLMs) like ChatGPT, Gemini, ClaudeAI, or LLAMA with existing information by copying relevant content into your clipboard. This tool is particularly useful for developers who frequently work in the shell and need to quickly provide context to LLMs when starting new chats.

## Features

- **Clipboard Copying**: Automatically copies the processed content to the clipboard, making it easy to paste into an LLM chat.
- **Zsh Autocomplete Support**: Utilizes Zsh autocomplete for fast and efficient prompt selection, allowing you to quickly choose the content you want to prime the LLM with.
- **Environment Variable Replacement**: Supports replacing environment variables within file paths, making it adaptable to different environments.
- **File Link Resolution**:
  - **Single File Links (`file://`)**: Embeds the content of a referenced file directly into the Markdown output.
  - **Multiple File Links (`files://`)**: Uses Node.js v22's globbing mechanism to resolve and embed content from multiple files matching a pattern.
- **Recursive Content Embedding**: Handles recursive inclusion of linked files, ensuring that even complex documents can be processed and included.
- **Context Priming**: Designed specifically for priming LLMs with relevant content, such as project source code, documentation, or any other necessary files, before starting a chat session.

## Installation

Save the script to a file, for example, `shellpromptor.js`, and make it executable:

```bash
chmod +x shellpromptor.js
```

Ensure that Node.js v22 or later is installed on your system, as the tool relies on modern features of Node.js.

## Usage

### Basic Usage

To process and copy a Markdown file's content to your clipboard:

```bash
./shellpromptor.js your_markdown_file
```

This will:
1. Search for `your_markdown_file.md` in the directories specified by the `PROMPTOR_PATH` environment variable.
2. Replace any `file://` and `files://` links within the Markdown content, potentially combining multiple files.
3. Copy the fully processed content to your clipboard, ready to be pasted into an LLM chat.

### Autocompletion Feature

To enable rapid prompt selection, you can list available Markdown files using the `--complete` flag:

```bash
./shellpromptor.js --complete
```

This will output a list of Markdown filenames found in the directories specified by `PROMPTOR_PATH`, which can be used for Zsh autocompletion.

### Setting Up Zsh Autocompletion

ShellPromptor includes a custom Zsh autocomplete function to make selecting prompts even faster. To enable this, follow these steps:

1. **Add the Autocomplete Function**: Add the following code to your `.zshrc` file:

   ```zsh
   #compdef promptor

   # Function to generate autocomplete options
   _promptor_autocomplete() {
     # Get the current word being typed
     local cur="${words[CURRENT]}"

     # Get the list of files from the `promptor` command
     local options=("${(@f)$(promptor --complete)}")

     # Use the Zsh built-in function `compadd` to suggest options
     compadd -a options
   }

   # Tell Zsh to use the _promptor_autocomplete function for promptor
   compdef _promptor_autocomplete promptor
   ```

2. **Source Your `.zshrc`**: After adding the above code, reload your shell configuration by running:

   ```bash
   source ~/.zshrc
   ```

3. **Use Autocompletion**: Now, when you start typing a command with `promptor`, pressing `Tab` will trigger the autocomplete, displaying available options based on the files listed by the `--complete` flag.

### Example Workflow

#### Step 1: Priming an LLM

1. Ensure your `PROMPTOR_PATH` is set to include directories containing your Markdown files.
2. Run ShellPromptor with the desired filename:

   ```bash
   ./shellpromptor.js project_overview
   ```

3. The content of `project_overview.md` (along with any linked files) is processed and copied to your clipboard.
4. Paste the content into your LLM chat to prime it with the necessary context.

#### Step 2: Utilizing Autocompletion

To quickly find and select a file:

```bash
./shellpromptor.js --complete
```

This outputs all Markdown files in your specified directories, allowing you to quickly select the correct one using Zsh autocomplete.

## Error Handling

- **File Not Found**: If the specified file is not found, an error message will be displayed, and the script will exit with a status code of 1.
- **PROMPTOR_PATH Not Set**: If the `PROMPTOR_PATH` environment variable is not set, the script will output an error and exit.
- **Recursion Limit**: The script includes a recursion limit (default is 99) to prevent infinite loops when processing linked files. If this limit is reached, an error will be thrown.

## License

This script is released as open-source under the [MIT License](LICENSE).
