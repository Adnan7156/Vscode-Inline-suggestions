# Groq Inline Suggestions

A VS Code and VSCodium extension that uses the [Groq API](https://console.groq.com/) to suggest code. It supports Python, Django, JavaScript, React, TypeScript and HTML by default.

## Features

- **Ghost-text suggestions:** See code completions at the cursor as you type.
- **Next-edit suggestions:** Request a localized edit and review a diff before applying it.
- **Snooze:** Pause automatic ghost-text suggestions for five minutes.
- **Configurable:** Choose a Groq model, completion delay, token limit and supported language IDs.
- **Secret storage:** Your API key is stored using the editor's Secret Storage API, not in extension settings or source code.

> Next-edit suggestions are currently **on demand**, not automatic predictions. The snooze and main on/off switch control ghost text; next edits have a separate setting.

## Requirements

- VS Code or VSCodium version **1.85 or newer**.
- A Groq account and API key.
- An internet connection and access to a supported Groq model.
- Node.js and npm if you are building the VSIX from source.

## Get a Groq API key

1. Go to [Groq Console](https://console.groq.com/) and sign in or create an account.
2. Open the [API Keys page](https://console.groq.com/keys).
3. Create an API key and copy it. Keep it private: do not commit it to GitHub or paste it into an issue.
4. After installing the extension, open the Command Palette and run **Groq Inline: Set API Key**. Paste your key into the password input.

Groq model availability, access, rate limits and pricing depend on your account. See the [Groq documentation](https://console.groq.com/docs/models) for available models.

## Install in VS Code or VSCodium

### From a VSIX file

1. Download `groq-inline-suggestions-1.1.1.vsix` from this repository's Releases page, if the repository owner has uploaded it there. Otherwise, build it using the instructions below.
2. Open **VS Code** or **VSCodium**.
3. Open the **Extensions** view.
4. Click the **…** menu at the top of the Extensions view and select **Install from VSIX…**
5. Select the downloaded VSIX file and reload the editor if prompted.
6. Run **Groq Inline: Set API Key** from the Command Palette and enter your Groq API key.

Alternatively, install from a terminal, replacing the path with the actual location of your VSIX:

```bash
# VS Code
code --install-extension ./groq-inline-suggestions-1.1.1.vsix

# VSCodium
codium --install-extension ./groq-inline-suggestions-1.1.1.vsix
```

### Build a VSIX from source

Clone this repository, open a terminal in its root folder, and run:

```bash
npm run package
```

This uses `npx` to run `@vscode/vsce` and creates a VSIX file in the current folder. Install it using the steps above. Packaging requires Node.js, npm and network access to download the packaging tool.

To try the source without packaging, open the repository folder in VS Code or VSCodium and press **F5** to start an Extension Development Host. Run **Groq Inline: Set API Key** in the new window.

## Use

### Ghost-text suggestions

1. Make sure **Editor: Inline Suggest Enabled** is turned on in the editor settings.
2. Open a supported file, such as a Python or React file, and begin typing.
3. Pause briefly for a suggestion. Press **Tab** to accept it or **Escape** to dismiss it.

Click **Groq: On/Off** in the status bar to toggle automatic suggestions. You can also run **Groq Inline: Snooze for 5 Minutes** or **Groq Inline: Resume Suggestions** from the Command Palette.

### Next edit

Open a supported file and run **Groq Inline: Suggest Next Edit**. Review the displayed diff and choose **Apply** to make the change, or dismiss the prompt to leave the file unchanged. If the file changes while the suggestion is being generated or reviewed, the extension discards the suggestion.

Use **Groq Inline: Toggle Next Edit Suggestions** to enable or disable the feature separately from ghost text.

## Settings

Search for **Groq Inline Suggestions** in the editor's Settings UI, or configure these in `settings.json`:

```json
{
  "groqInline.enabled": true,
  "groqInline.nextEditEnabled": true,
  "groqInline.model": "openai/gpt-oss-120b",
  "groqInline.delayMs": 600,
  "groqInline.maxTokens": 256,
  "groqInline.languages": [
    "python",
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact",
    "html",
    "django-html"
  ]
}
```

The default model is `openai/gpt-oss-120b`. If it is unavailable to your account, set `groqInline.model` to another supported Groq model ID. For Django templates, add the language ID provided by your template extension if it differs from those listed above.

## Troubleshooting

- **No ghost text:** Check that **Groq: On** is shown, the editor's inline suggestions are enabled, your file's language ID is in `groqInline.languages`, and your API key is set. Start typing and pause; an empty file does not trigger a request. Other completion extensions may also affect what you see.
- **Model not found or access denied:** Verify `groqInline.model` against models available to your Groq account. The older `llama-3.3-70b-versatile` default has been retired; change any explicit setting that still uses it.
- **API error:** Check your key, network connection, model access and Groq account limits. Run **Groq Inline: Set API Key** to replace the key.
- **Next edit unavailable:** Check that `groqInline.nextEditEnabled` is true, a supported file is open and an API key is set.

## Privacy and security

For ghost text, the extension sends the file's basename and up to 12,000 characters before and 4,000 characters after the cursor to Groq. For a next edit, it sends the basename and up to 201 lines around the cursor, capped at approximately 18,000 characters. Requests go to `api.groq.com`. Review your organization's policies and Groq's terms before using the extension with confidential code.

Generated code can be incorrect or insecure. Review suggestions before accepting or applying them. To remove your key, run **Groq Inline: Clear API Key**.

## Publishing this repository

Do not commit API keys, `.env` files, or other secrets. If you want users to install a prebuilt extension, attach the generated VSIX to a GitHub Release and update the download instructions to point to that release. The repository itself does not automatically create a release.
