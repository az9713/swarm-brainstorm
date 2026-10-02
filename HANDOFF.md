# HANDOFF — resume point for dot_vs_swarm_ethan

Read this file first in each new session. `~\.claude\CLAUDE.md` holds the standing rules (ASD-STE100, precise phrasing).

## Current state (verified 2026-10-01)
- **Skill:** `~\.claude\skills\swarm-brainstorm\SKILL.md` exists (global). It is one prompt (goal, search strategy, three evaluation views, "You may delegate"). It has no code.
- **First real use:** the skill ran twice on "Jev use cases that have not been widely discussed". Jev = TypeSafe AI's "System One" decision model. Run 1 used open web research. Run 2 used only two sources the user gave: the TypeSafe launch blog and `yibie/awesome-jev`.
  - Each run started 7 subagents: a gather/map stage, an ideate stage, then the parent judged with no agent. Run 1 split 3 + 4. Run 2 split 4 + 3. 14 agents, 1,393,665 subagent tokens in total.
  - Run 1 winner: all-pairs contradiction scan of a policy corpus (both option orders must say "contradict" at p of 0.7 or higher). Runner-up: clause-ambiguity index.
  - Run 2 winner: dead-letter-queue triage with a replay contract (only idempotent messages replay automatically). Runner-up: questionnaire pretest with synthetic respondents.
  - No candidate was built or tested. No Jev API call was made. All 35 candidates and the ranked rejected lists are in `brainstorm-results.html`.
- **Write-up:** `DEVELOPMENT-JOURNEY.html` (dark mode, sections 1–13). Section 5 holds the exact prompt and a discussion of each of the 14 agents. Section 6 covers agent-to-agent communication. Section 7 covers how the agent count was chosen. Section 4 has the stage tables.
- **Published:** public repository `az9713/swarm-brainstorm` on GitHub, branch `main`, with GitHub Pages serving the two HTML files. Files in the repository: `README.md`, `SKILL.md`, `brainstorm-results.html`, `DEVELOPMENT-JOURNEY.html`, `HANDOFF.md`, `.gitignore`.
- **History was rewritten before publishing.** The three earlier local commits contained raw terminal transcripts (`cc1`–`cc3`, `gpt6_summary.txt`) that the user had moved out of the root. To keep them out of the public history, `main` is a fresh single-commit orphan branch. The old commits remain only in the local branch `pre-publish-history`, which is not pushed.
- **Commits** use a neutral author (`anonymous <anonymous@users.noreply.github.com>`, passed with `git -c user.name=… -c user.email=…`) and omit the session URL, because the user asked to scrub personal info.
- **Ignored on purpose** (`.gitignore`): `*_files/` and `The Dot and the Swarm*.html` (the saved article embeds the logged-in reader's account data), `prompt.txt` (holds a private chat link; its prompt text is quoted in the README), `.ignore/` (the user's raw transcripts), `.impeccable/` (hook cache).
- **Personal-info scrub:** local paths became `~`, the machine name became `host`, the user name became "the user". Before any new commit, search the staged files for the user's real name, email handle, Windows user name, machine name, and Windows user-profile paths (expect no match). Do not write those strings into this file.

## Next task
- **Run the four planned tests of the skill** (still NOT done). In a fresh session, record 3 numbers for each (subagent count, distinct roles, whether the choice has reasons on all 3 views):
  1. `/swarm-brainstorm next OneUsefulThing post` (control).
  2. `/swarm-brainstorm name for a new skill that summarizes articles` (small task).
  3. `/swarm-brainstorm product ideas for a company that sells agent oversight tools` (large task).
  4. Test 1 again with the added sentence "Use brainstormers, researchers, and a panel of readers." (tests whether a team sketch raises the count).
- If tests 2 and 3 use equal counts, the model does not scale agents to the task. Then consider an edit to the skill.
- Known gap in both Jev runs: no agent applied the "reader perspective" view. The parent judged it. A reader-panel agent is an option for the skill.
- If the user asks for something else, that takes precedence.

## Where to read things
- `DEVELOPMENT-JOURNEY.html` — full account of the two runs: stages, agents, prompts, errors, costs.
- `~\.claude\skills\swarm-brainstorm\SKILL.md` — the skill.
- `README.md` — project description, the original and modified prompt, links to the live pages.
- `brainstorm-results.html` — all candidates, the two chosen ideas, the ranked rejected lists.
- `prompt.txt` (local only, gitignored) — the prompt Mollick quotes (article paragraph 23).
- `.ignore/` — the user's raw transcripts and the ChatGPT article summary (`gpt6_summary.txt`). Unchecked claim in that summary: it says the article gives "examples" of agents pursuing undesirable behavior. The article names only the Hugging Face Incident.

## Workflow provenance and evidence
- Skills used: `swarm-brainstorm` (2 runs), `dev-journey` (the journey document), `handoff-after-clear` (this file). Not called: the `advisor` tool and the `Workflow` tool.
- Last verified 2026-10-01: git state by `git status --short` and `git log`. Agent counts, tokens, and times from the 14 task-completion notices.
- Unknowns: whether `.ignore/` is meant to stay out of git; whether the user wants the rejected-candidate lists saved to a file; whether two runs on one task say anything about how the model chooses agent counts.

## Session-transient scratch (regenerate; the durable record is the committed HTML)
- Scratchpad scripts, lost after clear: `scrub.py` (plain `str.replace` of user path, machine name, name), `agents_data.py` (the 14 prompts and per-agent text), `build_agents.py` (inserted section 5 into the HTML, renumbered later sections, not idempotent). Edit `DEVELOPMENT-JOURNEY.html` directly instead of rebuilding.
- Local copy of `awesome-jev` (README + 16 category files, 399 KB) and pre-scrub backups of five files were in the scratchpad. They are lost after clear. Re-download the list with the GitHub API tree and `curl` of raw files if needed.
- The 14 agents' full reports are not saved anywhere.
