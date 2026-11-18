# Audio Perception 0.1.3 (experimental)

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
- Analyzes the whole file; scene constraints and acoustic adaptations are textual only, not actual remixing or timestamp clipping. No real-time microphone, reusable discography, cross-chat cache, or exact playback progression yet.
- On analysis failure, aborts that generation to avoid an uninformed response. Disable the extension and retry if necessary. The native audio attachment itself may still be retained by SillyTavern.
- Don't publish API keys, chat files, or audio recordings to the extension repository. Only publish the extension source files and this README. Back up your chats first. Disable or uninstall via the Extensions panel to revert to normal behavior.

## 0.1.1 change

Accepts M4A files, including browser MIME aliases audio/mp4 and audio/x-m4a, and passes their bytes unchanged to Gemini as audio/m4a. Native attachment acceptance and live Gemini decoding still depend on the installed SillyTavern version and Google backend. No changes to settings or cached analysis keys.

## 0.1.2 change

- More specific section-by-section lyrical analysis, concrete paraphrases, short audible fragments, and explicit uncertainty. No lyric lookup or reconstruction from memory.
- Roleplay prompt lets characters engage with specific lyrical ideas naturally without assuming autobiography or requiring a comment.
- View / edit most recent cached analysis opens an editor. Reanalyze uses the current Gemini model, incurs a new API request, and preserves the original cached entry until Save changes. Add your own lyrics or notes in the optional field; they are stored in chat metadata and sent as text to the roleplay model, not used for the Google reanalysis request. Close without saving discards changes. In 0.1.2 only the latest attachment was editable; 0.1.3 adds a browser for all cached audio in the current chat.
- Old cached observations are not automatically reanalyzed; use the editor to refresh them. Existing model and attribution settings remain intact.
- The analysis still depends on what Gemini can reliably hear; difficult sung words may be unintelligible. No live deployment test performed for this update.

## 0.1.3 change

- Upgrade the same Git repository by replacing the three files, then update the extension through SillyTavern and refresh. No server changes required. Keep the old zip as a rollback option.
- **Ask for per-attachment settings on Send** defaults to on. After attaching audio and sending, a dialog opens before analysis and roleplay generation. Use defaults skips individual edits; turn the setting off for automatic analysis. Cancel stops generation but keeps the sent message and native audio; regenerate to try again. Multiple new attachments are configured separately.
- Independent authorship, title, performer, presentation, arrangement, optional audible interval, notes and user-supplied lyrics are stored in the attachment metadata. Google analyzes anonymously in every mode; real/cover attribution relies on your supplied metadata and in-story knowledge, not automatic song identification.
- **Review analysis before roleplay generation** (off by default) allows editing the analysis and lyrical notes. Canceling this review does not cache the result; retrying will make another Google request.
- **Browse all cached audio in this chat** opens an editor for any analyzed attachment in this chat (not a cross-chat discography). Existing cached observations work without reanalysis. Editing the listening interval should be followed by reanalysis or matching manual edits.
- Optional fallback model: at most one second model request on HTTP 429/500/502/503/504, fetch transport errors or timeouts. SillyTavern can mask the underlying Google error, so a fallback may not help. Leave empty for no extra calls. Model actually used is recorded.
- Additional analysis priorities supplement the built-in anonymous analysis prompt. Listening interval is prompt-based: entire recording is still uploaded and precise boundaries are not guaranteed. Use a pre-trimmed attachment for strict timing. Arrangement adaptation is textual only.
- No media bytes, lyrics or per-attachment analyses go into global settings; the optional user-specified default text fields are small and bounded by UI length limits. No startup audio downloads or analysis requests. Existing cache key is unchanged.
- Validation: Node syntax check and mocked browser/API/interceptor tests (dialog selection, review, cancellation, fallback, older cache, saving edits). Not tested live with SillyTavern, Render, or Gemini.
