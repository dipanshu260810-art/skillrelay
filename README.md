# SkillRelay

**[Open SkillRelay in your browser](https://dipanshu260810-art.github.io/skillrelay/)** · [Get the offline app](./SkillRelay.html)

Have a skill you want to use in another assistant? SkillRelay reads the skill on your device and prepares a ZIP for Claude or ChatGPT. You can also copy instructions into a regular chat. You can use the converter and its sample without an account, subscription, or installation.

To keep an offline copy, open **Get the offline app**, click **Download raw file** on GitHub, and open the downloaded HTML file in a modern browser.

## Try a skill

1. Choose **Try sample**. SkillRelay converts a meeting-summary skill and shows the result immediately.
2. Read the compatibility report. For the sample, look for the manual-invocation setting and the Claude-specific argument hint. Open **Package contents** to see the instructions and the meeting brief template that travels with them.
3. For a regular chat, select **Include linked reference text** to include the meeting brief template. Review the preview, choose **Copy instructions**, and paste it into a chat with your meeting notes. For a workspace with skill uploads, download the ZIP and follow the relevant setup below.

For your own project, choose a direction and supply one skill ZIP, a standalone `SKILL.md`, or pasted skill Markdown. Use the ZIP when a skill relies on files such as templates or scripts.

## Where to use the output

| Destination | What to do |
| --- | --- |
| ChatGPT | For eligible Business, Enterprise, Healthcare, and Edu workspaces with uploads enabled, use **Plugins → Skills → Add → Upload from your computer**. Pasting instructions works in a regular chat. [OpenAI’s Skills guide](https://help.openai.com/en/articles/20001066-skills-in-chatgpt) |
| Codex | Unzip the skill folder into `.agents/skills/` for your project. [OpenAI’s Codex skill locations](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills) |
| Claude | Anthropic lists Free, Pro, Max, Ultra, Team, and Enterprise. An eligible user with code execution and file creation enabled can upload the ZIP from **Customize → Skills**. Organization settings may also apply. [Anthropic’s Skills guide](https://support.claude.com/en/articles/12512180-use-skills-in-claude) |
| Claude Code | Unzip the skill folder into `.claude/skills/` for your project. [Anthropic’s skill locations](https://code.claude.com/docs/en/skills#choose-where-skills-load) |

Claude consumer accounts require users to be at least 18. You can practice converting in either direction using SkillRelay’s bundled sample entirely in your browser. [Anthropic minimum age](https://support.claude.com/en/articles/8114491-get-started-with-claude)

## Pass work between assistants

In **Chat handoff**, pick a direction, add a task and the context you want to share, and preview the result. Copy it into the receiving chat, or download the JSON to reopen it later. Share the resulting reply back by reversing the direction and adding the excerpt you choose.

Two local clients that support the Model Context Protocol (MCP) can also exchange handoffs through a shared folder. SkillRelay provides `send_handoff` and `read_handoff`. Each user signs in to their own assistant and selects what to share. The source package includes setup examples in `docs/handoff.md`.

## Build from source

Download and extract the [source archive](https://github.com/dipanshu260810-art/skillrelay/blob/main/SkillRelay-source.zip). If you already have a source checkout, use that folder. With Node.js 22 or newer, run these commands from the folder containing `package.json`.

```sh
npm ci
npm test
npm run build
```

Open `dist/skillrelay.html` to run the offline build. Use `npm start` for a local preview at `http://127.0.0.1:4173`. For static hosting, serve the generated `dist/` directory.

## Command line

Start with the example Markdown or pass a ZIP if the skill has supporting files.

```sh
node bin/skillrelay.mjs convert examples/meeting-brief/SKILL.md --to chatgpt
node bin/skillrelay.mjs convert meeting-brief-for-chatgpt.zip --to claude
npm test
```

Use `--out <file.zip>` to choose a destination. The command reports adaptations and review items in the terminal. Existing output files are protected from accidental overwrites.

## What the converter changes

- Normalizes the skill name and folder, validates the shared frontmatter, and keeps the Markdown instructions and supporting file contents. Export to Claude also normalizes reserved provider words in the name.
- Preserves the manual-only invocation setting between Claude Code and Codex, so a skill that requires an explicit request can keep that behavior. Check the destination’s controls when installing into another client.
- Reports other host-specific settings, such as Claude Code hooks, preapproved tools, and tool dependencies, for review. It flags some common tool names, Claude Code substitutions, missing linked files, and bundled scripts. Keep the original bundle and recreate any required settings in the destination before testing the skill.
- Accepts up to 256 ZIP entries, 24 MiB compressed, and 20 MiB unpacked, with a 10 MiB per-entry limit. `SKILL.md` is limited to 512 KiB. Password-protected ZIPs, ambiguous paths, links and special files are rejected.

The converter runs locally and treats skill files as data. It does not run scripts or open links from a skill. The compatibility report is a format check plus a few migration checks. Inspect what the skill tells an assistant to do before enabling it. Convert and redistribute skills you have permission to use.

## Contributing

If a skill loses a file or a step in conversion, [open an issue](https://github.com/dipanshu260810-art/skillrelay/issues). Include the source and destination, your browser, what you expected, and a small example you have permission to share. Replace private notes and credentials with sample values.

For a code change, run `npm test` and `npm run build` from the source folder. A good conversion test starts with a small skill, converts it, opens the resulting ZIP, and checks the instructions and supporting files. Run the case in both directions when it applies to both. [Agent Skills specification](https://agentskills.io/specification)

The initial implementation was built with AI assistance. Code and the example are available under the [MIT license](LICENSE).
