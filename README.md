# Audio Perception 0.1.2 (experimental)

A client-side SillyTavern extension targeting the 1.14+ native audio attachment workflow. It uses SillyTavern's authenticated Google AI Studio backend (not a new server or an exposed key). Default off. No dependencies.

## Installation

1. Confirm SillyTavern 1.14+ and that its native attachment control accepts audio files. Configure a Google AI Studio key in API Connections; then switch back to your usual API or connection profile.
2. Put `manifest.json` and `index.js` at the root of a Git repository (not inside another folder). Install the repository URL through Extensions > Install extension, selecting the current user if offered. Alternatively install the whole `audio-perception` folder in the current user's `extensions` directory under SillyTavern's configured data root. For Render, ensure the data directory is on the persistent disk.
3. Reload SillyTavern. Open Extensions > Audio Perception (Prototype), enable it, choose an available Gemini audio-input model ID, and select attribution and presentation. Default model for new installations is `gemini-3.5-flash-lite`; existing saved settings are unchanged. If that model isn't supported by your deployed backend/Google project, enter another audio-capable Flash model from your Google model list.
4. In a disposable new chat, attach a short MP3, WAV or M4A using native attachment controls and send with a narrative message. Use the extension's View / edit most recent cached analysis button to inspect results. Swipe the answer and check that no second analysis notification appears.

## Limitations

- Tested only with JavaScript syntax checks and a mocked interceptor test, not a live SillyTavern deployment or Google API. API and UI compatibility may vary.
- Supports local server MP3/WAV/M4A attachments under 12 MiB. The extension sends audio bytes to Google through the existing backend; provider usage limits, privacy policies and charges apply. No real-world filenames are deliberately added to the analysis prompt. Embedded tags are not stripped in this prototype; identity leakage remains possible. Rename files to a neutral name and strip tags separately if desired.
- Cached analysis is attached to message media metadata; it is injected into prompt copies, not the visible narrative. Old, unanalyzed attachments are not automatically backfilled. Changing settings does not rewrite cached entries. The original media remains in chat storage. The extension removes native audio from prompt copies when enabled; existing native caption/title behavior may differ across versions.
- Analyzes the whole file; scene constraints and acoustic adaptations are textual only, not actual remixing or timestamp clipping. No real-time microphone, discography UI, cross-chat cache, or exact playback progression yet.
- On analysis failure, aborts that generation to avoid an uninformed response. Disable the extension and retry if necessary. The native audio attachment itself may still be retained by SillyTavern.
- Don't publish API keys, chat files, or audio recordings to the extension repository. Only publish the extension source files and this README. Back up your chats first. Disable or uninstall via the Extensions panel to revert to normal behavior.

## 0.1.1 change

Accepts M4A files, including browser MIME aliases audio/mp4 and audio/x-m4a, and passes their bytes unchanged to Gemini as audio/m4a. Native attachment acceptance and live Gemini decoding still depend on the installed SillyTavern version and Google backend. No changes to settings or cached analysis keys.

## 0.1.2 change

- More specific section-by-section lyrical analysis, concrete paraphrases, short audible fragments, and explicit uncertainty. No lyric lookup or reconstruction from memory.
- Roleplay prompt lets characters engage with specific lyrical ideas naturally without assuming autobiography or requiring a comment.
- View / edit most recent cached analysis opens an editor. Reanalyze uses the current Gemini model, incurs a new API request, and preserves the original cached entry until Save changes. Add your own lyrics or notes in the optional field; they are stored in chat metadata and sent as text to the roleplay model, not used for the Google reanalysis request. Close without saving discards changes. Only the latest analyzed attachment in the current chat is editable in this version.
- Old cached observations are not automatically reanalyzed; use the editor to refresh them. Existing model and attribution settings remain intact.
- The analysis still depends on what Gemini can reliably hear; difficult sung words may be unintelligible. No live deployment test performed for this update.
