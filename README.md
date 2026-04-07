# colas-zola-tools
The scripts I use to maintain my zola site.
Nothing fancy or groundbreaking, I publish the ones that may be useful to others. Script are standalone and in the `bin/` subdirectory.

All my personal site management tools start with the prefix `site-` for taking advantage of the tab-completion. They should run on any Linux or Unix system with the GNU utilities (grep, sed...)

## site-toml2yaml

Since [Zola](https://www.getzola.org/) now supports frontmatter in its markdown pages in the [YAML](https://yaml.org/) format in addition to the original [TOML](https://toml.io/), I decided to migrate all my pages to YAML, as it is both easier to read and write for humans, and natively understood by a majority of tools (Emacs, marktext, obsidian, vs-code, typos, zettlt...)

So I wrote a small bash script for the conversion, `site-toml2yaml` (all my personal site management tools start with the prefix `site-` for taking advantage of the tab-completion. 
It should run on any Linux or Unix system, and it requires [dasel](https://github.com/TomWright/dasel) and the usual gang: grep, sed, head...

Note that you could also use **[hugo convert](https://gohugo.io/commands/hugo_convert/)**, but I haven't tested it.

You can grab it at my [zola-tools repository](https://github.com/ColasNahaboo/colas-zola-tools). This repository is empty for now, but I will use it to publish the various small tools I use to maintain this public site.

### Usage

**site-toml2yaml** *md-files or directories...*

Converts in place the frontmatter of Markdown files from TOML to YAML.\
It recurses in directories, but looking only for `*.md` files and sub-dirs.\
Without argument, run on `${ZOLA_ROOT}/content/` \
Should work on any Markdown files with TOML frontmatter.

You should only run it on your content/ dir normally. For instance running it
on your themes/ dir will risk messing your imported themes

WARNING: it modifies the files in place, so MAKE A BACKUP before running it!

### Why converting to YAML?

TOML is arguably a [better configuration file format](https://npf.io/2014/08/intro-to-toml/) than YAML, but only if you want to use it as a simpler JSON, i.e. a well-specified, unambiguous way to represent any data.

However, I feel that for the simple and limited use case of Zola pages frontmatter, it is overkill and thus a real pain to read and edit by hand. Let's take an example:

#### TOML

```toml
[table1]
	foo = "bar"

	[table1.nested_table]
		baz = "bat"

[[comments]]
author = "Nate"
text = "Great Article!"

[[comments]]
author = "Anonymous"
text = "Love it!"
```

#### YAML

```yaml
comments:
  - author: Nate
    text: Great Article!
  - author: Anonymous
    text: Love it!
table1:
  foo: bar
  nested_table:
    baz: bat
```

See? Especially for people like me with a Unix culture, the YAML format seems both more natural (resembling the format of email or HTTP headers), and TOML has a repulsive Windows flavor. Add to this that most of the tools I use understand natively YAML frontmatter but not TOML, and the decision is easy.
