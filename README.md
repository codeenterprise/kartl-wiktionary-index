# kartl-wiktionary-index

Ein maschinell gefilterter Teilbestand des Wiktionary: **Stichwort →
Übersetzungen**, sonst nichts. Er liegt hier, weil die
[kartl](https://github.com/johanndrews/kartl)-App ihn mitliefert und
CC BY-SA 4.0 ihn damit selbst unter CC BY-SA 4.0 stellt. Je App-Version gibt es
ein getaggtes Release. Dieses Repo bleibt öffentlich, solange kartl vertrieben
wird.

---

A machine-filtered subset of Wiktionary: **headword → translations**, and
nothing else. It lives here because the
[kartl](https://github.com/johanndrews/kartl) app ships it, and because
CC BY-SA 4.0 puts the resulting index under CC BY-SA 4.0 as well.

## Status: no index files yet

**This repository currently holds only `LICENSE` and this `README.md`.** The
index files arrive with the first tagged release, which is published together
with kartl 1.0.0. The repository was created ahead of that release so that the
licence, the source chain and the modification notice are a matter of public
record before any data is distributed — not afterwards.

If you are looking for data and find none, nothing is broken and nothing is
being withheld: there is no released kartl version yet whose index would belong
here.

## License

**The data in this repository is licensed under
[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).** The full
license text is in [`LICENSE`](LICENSE) — not just a link to it.

This covers the `index-*.json` files. It does not cover the script that builds
them: `Tools/wiktionary-index.py` in the kartl repository is a separate work
and contains no dictionary data.

**Disclaimer of warranties.** The index is provided as-is, without warranties
or conditions of any kind, as set out in CC BY-SA 4.0 Section 5. Wiktionary is
written by volunteers; translations can be wrong, incomplete, regional or
outdated. kartl uses the index to *suggest* words while someone types them. A
suggestion is never applied silently and never overwrites a value the app read
from a printed book.

## Why this repository exists at all

Whether the index carries its own database right is genuinely arguable — the
investment is a free download plus a script and some CPU time, and CC BY-SA 4.0
§2(a)(4) says machine filtering does not by itself create *Adapted Material*.

We publish it anyway, as a precaution. Under the stricter reading, §4(b) of the
license applies and ShareAlike reaches the index. Publishing costs one
repository and settles the question permanently, for everyone who comes after
us. ShareAlike is met by a place where the material actually is, not by an
intention to share it.

## Attribution — the full source chain

| Step | Who / what | License | Where |
|---|---|---|---|
| 1. The content | The editors of the respective Wiktionary language editions | CC BY-SA 4.0 (text content; historically also GFDL) | [en](https://en.wiktionary.org) · [de](https://de.wiktionary.org) · [fr](https://fr.wiktionary.org) · [es](https://es.wiktionary.org) |
| 2. The extraction | `wiktextract` by Tatu Ylonen | MIT (tool) | [github.com/tatuylonen/wiktextract](https://github.com/tatuylonen/wiktextract) |
| 3. The dumps | kaikki.org, which hosts the generated JSONL dumps | data as per source | [kaikki.org/dictionary](https://kaikki.org/dictionary/) |
| 4. The filter | `Tools/wiktionary-index.py` in kartl | see that repository | [github.com/johanndrews/kartl](https://github.com/johanndrews/kartl) |

Per Wikimedia's own reuse guidance, attribution may be given as a link back to
the source rather than a list of every author. Step 1 is that link. Each
release notes the exact dump files and their dates, so the state of the source
at the time of extraction stays identifiable.

## What we changed — the modification notice

Required by CC BY-SA 4.0 §3(a)(1)(B). The index is **not** a copy of
Wiktionary. It is the result of these operations, all mechanical:

**Kept** — the headword, and its translations into the target language. From
two places in the dump: `senses[].translations` and the top-level
`translations` field, because individual entries fill one but not the other.

**Dropped** — definitions, etymology, pronunciation, inflection tables,
example sentences, usage notes, categories, and every language other than the
pair being built. This is the bulk of the source: a 3 GB dump becomes a file
of a few megabytes.

**Added by dereferencing, not by writing** — Wiktionary carries translations
on the lemma, not on the inflected form: `cat` has a translation table, `cats`
does not. Where the dump itself points from a form to its lemma
(`form_of`, `alt_of`, `forms`), the form inherits the lemma's translations.
Exactly one step, no chains. A form is only taken if its lemma itself has a
translation and the form is not already a headword of its own — an inherited
entry never overwrites a real one. Where several lemmas point at the same form
(`leaves` is both a plural of `leaf` and a form of `leave`), their
translations are merged, not replaced.

**Dropped from the inherited forms only** — periphrastic comparatives and
superlatives (`more unimaginable`, `most injudicious`) and forms containing a
character outside `[a-z0-9' .-]`. These are noise for the purpose of
completing a word someone is typing. The filter is applied **only** to
inherited forms, never to real headwords: `café`, `cliché`, `piñata` and
`least weasel` are vocabulary, and a word with its own Wiktionary translation
table is legitimate however it is spelled.

**Not done, at any point** — no translation was written, corrected, rated,
reordered by quality or invented by us. We removed and we dereferenced. We did
not author.

## Files

`index-<source>-<target>.json`, one file per direction. A JSON object, UTF-8,
non-ASCII characters unescaped:

```json
{"oven": ["Backofen", "Ofen", "Herd"], "cats": ["Katzen"]}
```

Keys are headwords, values are the translations in the order Wiktionary lists
them. The order carries no quality judgement of ours.

Directions: `en-de`, `fr-de`, `es-de` and the German side `de-en`, `de-fr`,
`de-es`. Latin is deliberately absent — no extractor covers it (kartl decision
of 2026-09-07).

## Versions — which data is in which app version

**One tagged release per app version.** Tag `v<app version>`: the release
tagged `v1.0.0` holds the index that ships inside kartl 1.0.0. Without that
mapping nobody can check which data sits in a given build.

**The measured figures live in the release notes, not here** — headword and
translation counts per direction, file sizes, and the dump file names with
their dates. A number repeated in this README would quietly go out of date;
there is one place for it instead.

## This repository stays public

As long as kartl is distributed in any app store, this repository stays
publicly reachable, and every release tagged for a distributed app version
stays downloadable. That is an obligation with a runtime, not a single act.

## Questions, license concerns, takedown requests

Open an issue at
[github.com/johanndrews/kartl/issues](https://github.com/johanndrews/kartl/issues).
Anything touching this index is handled there, in the open.
