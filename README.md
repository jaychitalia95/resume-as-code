# resume-as-code

A small template for treating your resume like code: write it once in
[Typst](https://typst.app), keep it in git, and let CI compile and publish the
PDF every time you change it. No more wrestling a Word document, no more
"final_v3_actually_final.pdf".

The point of this repo is the **pipeline**, not the design. The sample resume is
deliberately plain. Swap in your own layout; the build, versioning, and publish
flow stay the same.

## What you get

- **One source file** (`resume/resume.typ`) you edit in any text editor.
- **A spacing system + an `entry()` helper** so every job/role block lines up the
  same way instead of being hand-spaced.
- **A version stamp** on the PDF footer, fed by the git tag.
- **A GitHub Actions pipeline** that, on every push to `main` touching the
  resume: bumps a version tag, compiles the PDF with Typst, and attaches it to a
  GitHub Release. No secrets required.

## Quickstart

1. Click **Use this template** (or clone the repo).
2. Edit `resume/resume.typ` with your details.
3. Build it locally to preview:

   ```bash
   # one-time: install the pinned Typst version (https://mise.jdx.dev)
   mise install
   # or install Typst yourself, then:
   typst compile resume/resume.typ cv.pdf
   ```

4. Push to `main`. The Actions run tags a new version, builds the PDF, and
   publishes it under **Releases**.

## How versioning works

The pipeline uses [`github-tag-action`](https://github.com/anothrNick/github-tag-action)
to bump a `v*` tag on each qualifying push. The default bump is a patch
(`v0.0.1` -> `v0.0.2`). Put `#minor` or `#major` in your commit message to bump
those instead. The new tag is passed into the build (`--input version=...`) and
printed in the PDF footer, so every published resume says exactly which version
it is. The PDF itself is a build artifact and is gitignored.

## A note on ATS

Applicant tracking systems read your PDF by extracting its text, so a PDF is
"ATS-friendly" when that extraction comes out clean and in the right order. That
is a property of your *layout*, not of Typst. Single column, real selectable
text, standard section headings, no text hidden inside tables or graphics.

The honest test: open your compiled PDF, select all, copy, and paste into a plain
text editor. If it reads top to bottom the way a human would read it, an ATS will
probably handle it too. If the columns interleave or text comes out scrambled,
simplify the layout. The sample here is single-column on purpose.

## Hosting the latest at a stable URL (optional)

GitHub Releases give you a versioned archive, but each release has its own
download URL. If you want one permanent "latest" link to put on a website, push
the PDF to object storage too. `.github/workflows/resume.yml` has a commented-out
Cloudflare R2 step you can adapt to any S3-compatible bucket.

## License

[MIT](LICENSE). Use it however you like.
