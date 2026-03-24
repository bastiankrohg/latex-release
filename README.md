# Auto-build LaTeX → PDF

Every push to `main` compiles the LaTeX project and publishes the PDF as a rolling GitHub Release tagged `latest`. The release is automatically replaced on each new push, so there is always exactly one up-to-date PDF available.

---

## Quick setup

1. **Move `build.yml`** into the correct location in your repository:
   ```
   .github/workflows/build.yml
   ```
   (In the template it lives in the root for easy access — you need to move it into `.github/workflows/`.)

2. **Configure the three variables** at the top of `build.yml`:

   | Variable | Default | Description |
   |---|---|---|
   | `ROOT_FILE` | `main.tex` | Your top-level LaTeX file |
   | `OUTPUT_PDF` | `report.pdf` | Filename shown in the release |
   | `RELEASE_TAG` | `latest` | Git tag used for the rolling release |

3. **No secrets needed.** The workflow uses the built-in `GITHUB_TOKEN`, which has write access thanks to the `permissions: contents: write` block already in the file.

4. **Push to `main`** — the action will run automatically.

---

## Changing the trigger branch

The workflow fires on pushes to `main`. If your default branch is named differently (e.g. `master`), update this line in `build.yml`:

```yaml
branches:
  - main   # ← change this
```

---

## Finding the built PDF

Go to your repository on GitHub → **Releases** → **Latest Build**. The PDF is listed under *Assets*.

You can also link directly to the latest PDF using the release asset URL:

```
https://github.com/<user>/<repo>/releases/latest/download/<OUTPUT_PDF>
```

Replace `<user>`, `<repo>`, and `<OUTPUT_PDF>` with your values.

---

## Troubleshooting

| Symptom | Fix |
|---|---|
| `403` on release creation | Make sure `permissions: contents: write` is present in `build.yml` |
| Compile error | Check the *Actions* tab for the full LaTeX log |
| Wrong branch not triggering | Update `branches:` in `build.yml` to match your branch name |
| Missing packages | The `xu-cheng/latex-action` image includes most CTAN packages; add `extra_packages` to the step if needed (see the [action docs](https://github.com/xu-cheng/latex-action)) |
