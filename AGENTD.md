# Agent guide

This repository is the source for [alfurka.github.io](https://alfurka.github.io), a Jekyll
site built with the Beautiful Jekyll theme. Read this file before changing the site.

## Repository map

- `index.html` is the home page.
- `blog/index.html` is the paginated blog listing.
- `research.md` is the research and publications page.
- `tags.html` lists every post tag.
- `_posts/` contains posts named `YYYY-MM-DD-title.md`.
- `CV/cv.tex` is the editable CV source.
- `CV/cv.pdf` is the public compiled CV, and `CV/cv.xdv` is a tracked XeLaTeX
  intermediate output.
- `_config.yml` contains the Jekyll configuration, navigation links, defaults, and
  plugins.

## Post tags and the home page

Post tags are case-sensitive. Use the exact classification tags below in a post's YAML
front matter:

| Tag | Use for | Text shown in `index.html` |
| --- | --- | --- |
| `NEWS` | Personal or professional news such as publications, talks, awards, grants, or a new position. | `post.subtitle` |
| `blog` | Blog articles, technical notes, opinions, tutorials, or other longer-form posts. | `post.title` |

The two home-page loops are intentionally different:

```liquid
{% for post in site.tags.NEWS limit:4 %}
  {{ post.subtitle }}
{% endfor %}

{% for post in site.tags.blog limit:4 %}
  {{ post.title }}
{% endfor %}
```

Each matching post is linked to its full post and is followed by its date. The home page
shows up to four posts in each section. A post may have both `NEWS` and `blog`; it will
then appear in both sections, using the appropriate field in each section.

There is no home-page truncation for these fields. Keep the displayed value short enough
to read as a compact link, preferably a single brief phrase:

- For `NEWS`, make `subtitle` the short home-page announcement. The longer `title`
  remains useful on the post page and in metadata.
- For `blog`, make `title` short because it is the home-page link. Use `subtitle` for
  an optional explanation on the blog listing or post page.

The blog listing in `blog/index.html` displays both `title` and `subtitle` when a
subtitle exists. The tag index in `tags.html` displays the post title.

## Updating and compiling the CV

Edit `/home/runner/work/alfurka.github.io/alfurka.github.io/CV/cv.tex`, not the generated
PDF or XDV file. Keep the CV current when a task changes CV-relevant information,
including employment, education, publications, working papers, grants, talks, awards,
supervision, or skills.

After changing `cv.tex`, compile it from the CV directory:

```bash
cd /home/runner/work/alfurka.github.io/alfurka.github.io/CV
latexmk -xelatex cv.tex
```

The command uses `CV/.latexmkrc` and should refresh `cv.pdf` and `cv.xdv`. Check that
the PDF was produced successfully and include changed generated files in the same
change. `CV/CV_20260521.docx` is a separate Word copy/reference; update it only when
the request specifically requires the Word version.

Do not recompile the CV for an unrelated site or blog-only task. If the request is
CV-relevant, updating the source without compiling the public PDF is incomplete.

## Maintaining `research.md`

`/home/runner/work/alfurka.github.io/alfurka.github.io/research.md` is a manually
maintained Markdown page, not a file generated from the CV or from post front matter.
Keep it as the concise public research record:

1. Preserve the YAML front matter (`layout: page` and the page title).
2. Keep the short research-interest introduction.
3. Add or update entries under the appropriate heading:
   `Published Work`, `Working Paper`, `Grants`, or `Work-in-progress`.
4. For papers, record the title, year, outlet or status, co-authors, and stable links
   such as DOI, arXiv, SSRN, software, poster, blog post, or video links.
5. For grants, record the project, role, dates, amount when public, and investigators.
6. Keep entries concise and consistent with the existing Markdown link style.

When a task changes a publication, working paper, grant, or research project, update
`research.md` when the change belongs in this public summary. If the same information
belongs in the CV, update `CV/cv.tex` as well and compile the CV. A news post alone does
not automatically replace either research record.

## Agent task checklist

For every task, first inspect the relevant existing files and then:

- update a post's `NEWS`/`blog` tag and its displayed `title` or `subtitle` when the
  request creates or changes a post;
- keep the home-page field short according to the rules above;
- update `research.md` for research-record changes;
- update and compile the CV for CV-relevant changes;
- run the relevant existing Jekyll or CV validation command when the required tools are
  available;
- avoid changing unrelated files and verify generated artifacts before finishing.

For a local site build, install the gems if necessary and run:

```bash
cd /home/runner/work/alfurka.github.io/alfurka.github.io
bundle install
bundle exec jekyll build
```
