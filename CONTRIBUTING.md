# Contributing a build

Every build is one AI model's answer to the same prompt: [`prompt.txt`](prompt.txt).
The site lists every model in [`models.json`](models.json) with each of its effort
levels. Any effort without a build is an open slot, and anyone can fill it.

## Add a build

1. **Pick a slot.** On the site, dashed chips like `+ High` are builds nobody has made yet.
2. **Run the prompt.** Give the model `prompt.txt` exactly as written, at the effort level you picked.
   Don't edit the output by hand. If it needs follow-up turns, say so in your PR.
3. **Save it in a folder named `<lab>/<model>-<effort>/`:**

   ```
   openai/gpt-6-luna-high/
   ├── index.html   ← the model's single-file game
   └── prompt.txt   ← a copy of the root prompt.txt
   ```

   - `<lab>` is a lab `id` from `models.json`: `anthropic`, `google`, `openai`.
   - `<model>` is the model's `slug` from `models.json`, e.g. `opus-5.5`, `gemini-3.5-flash-lite`, `gpt-6-luna`.
   - `<effort>` is one of `none`, `minimal`, `low`, `medium`, `high`, `xhigh`, `max`, `ultra`.

4. **Check it** (optional, Node 18+):

   ```sh
   node scripts/sync-builds.mjs --check
   ```

5. **Open a pull request.** You don't need to edit `models.json` or `models.js`: once your PR
   is merged, a GitHub Action adds your folder to both and the build appears on the site.

Want credit on the card? Add `"by": "<your-github-username>"` to your build's entry in
`models.json` after it's synced.

## Add a model or a lab

Models and labs live in `models.json`. Add the model to its lab's `models` list (or add a new
lab with an `id`, `name` and `color`) in the same PR as your build:

```json
{ "slug": "gpt-6-luna", "name": "GPT-6 Luna", "position": "Efficient / high-volume", "efforts": ["none", "low", "medium", "high", "xhigh", "max"] }
```

Use `"efforts": "configurable"` when a model doesn't have fixed effort levels.

`models.js` is generated from `models.json` (plus file stats and the prompt). Never edit it by hand;
run `node scripts/sync-builds.mjs` to regenerate it.

## Run the site locally

It's static HTML. Open `index.html` in a browser, or serve the repo root:

```sh
node scripts/sync-builds.mjs   # after adding or renaming build folders
python3 -m http.server         # run from the repo root, then open http://localhost:8000
```
