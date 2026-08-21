# Nguyen Thanh Dat

Software engineer in Thanh Hoa, Vietnam. I work on AI gateways — the proxies that sit between
a coding agent and three hundred model providers, where a small correctness bug becomes every
user's bug. I read the issue nobody has picked up, reproduce it, and follow it to the line
that is actually wrong.

**English** · [Tiếng Việt](README_vn.md)

---

## Merged upstream

Five pull requests merged into [OmniRoute](https://github.com/diegosouzapw/OmniRoute), an MIT
AI gateway fronting 340 providers. Four close an issue someone else reported. Each carries a
regression test that fails on the base branch and passes with the change.

| Pull request | What it fixes |
| --- | --- |
| [#10843](https://github.com/diegosouzapw/OmniRoute/pull/10843) `fix(security)` | An SSRF guard that matched cloud-metadata hosts by spelling instead of by address. Detailed below. |
| [#10860](https://github.com/diegosouzapw/OmniRoute/pull/10860) `fix(mcp)` | One hardcoded fetch budget covered every internal server-to-server hop, so provider-bound tool calls inherited a timeout meant for something else. Closes [#9717](https://github.com/diegosouzapw/OmniRoute/issues/9717). |
| [#10858](https://github.com/diegosouzapw/OmniRoute/pull/10858) `fix(context)` | Base64 documents were measured character by character, so a 1 MB PDF estimated at 350,022 tokens and the request was rejected before it ever left. Closes [#10840](https://github.com/diegosouzapw/OmniRoute/issues/10840). |
| [#10857](https://github.com/diegosouzapw/OmniRoute/pull/10857) `fix(catalog)` | With auto routing off, `/v1/models` still advertised every `auto/*` id that the router would reject at request time. Closes [#10831](https://github.com/diegosouzapw/OmniRoute/issues/10831). |
| [#10853](https://github.com/diegosouzapw/OmniRoute/pull/10853) `fix(i18n)` | Eight locales rendered the *status* "Disabled" as the noun for a person who has a disability. Closes [#10812](https://github.com/diegosouzapw/OmniRoute/issues/10812). |

+777 / −32 across 26 files, merged into `release/v3.8.50`.

---

## One of them, in detail

`isCloudMetadataHost()` guards the classic SSRF pivot: an attacker who can steer an outbound
request at `169.254.169.254` reads the instance's IAM credentials. The code documented this
block as unconditional. It was not.

The guard compared the hostname against a set of dotted-decimal strings. But WHATWG `URL`
serialises an IPv4-mapped IPv6 literal as hextets, so the same address arrives wearing a
different spelling:

```
http://[::ffff:169.254.169.254]/
        │
        └─ new URL(…).hostname  ─►  "::ffff:a9fe:a9fe"
                                     │
                                     ├─ in CLOUD_METADATA_HOSTNAMES?  no
                                     └─ startsWith("169.254.")?        no
                                                                       │
                                            routes to 169.254.169.254 ─┴─►  allowed
```

The fix folds the embedded IPv4 back out before deciding — `a9fe` and `a9fe` are two hextets
that have to be decoded to `169.254.169.254`, not string-matched — so the verdict follows the
address rather than its spelling. Reading the surrounding function turned up a second gap:
`::` is the IPv6 twin of `0.0.0.0` and reaches a service on the IPv6 loopback, but only the
IPv4 spelling was refused. That is in the same patch.

Evidence I reported with it: the new test fails 10 of 12 cases on `release/v3.8.50` and passes
12 of 12 with the change; the guard's five existing suites stay green at 73 of 73.

I reported this privately first, as the project's `SECURITY.md` requires, and opened the pull
request against the active release branch once the maintainers had it.

---

## Security research

Four vulnerabilities reported to the OmniRoute maintainers through private advisories.

The one above is public because its fix is merged and awaiting the `v3.8.50` tag. The other
three are still in triage and unpatched, so there are no details here — they stay private
until the maintainers ship a fix. One of them is a good deal more serious than the one I have
described.

---

## How I work

Three things I would rather be judged on than a language list. Each links to the artefact.

**I report what the tests actually said.** I could not get a green build on my Windows box for
[#10843](https://github.com/diegosouzapw/OmniRoute/pull/10843#issuecomment-5355999035), so
instead of ticking the box I showed the identical failure on a clean checkout of the base
branch, traced it to an optional native dependency, and said plainly that a real CI run would
be more authoritative than mine.

**I widen a report when the bug is wider.** [#10812](https://github.com/diegosouzapw/OmniRoute/issues/10812)
reported one bad Japanese string. The same mistranslation was in eight locales and 24 strings,
so [#10853](https://github.com/diegosouzapw/OmniRoute/pull/10853) fixed all of them and added
glossary entries so the next translator does not repeat it.

**I stand down when someone was there first.** I opened
[9router#3434](https://github.com/decolua/9router/pull/3434) eleven minutes after
[#3433](https://github.com/decolua/9router/pull/3433) fixed the same defect, having missed it
while checking for duplicates. I closed mine, said why, and
[left the two regression cases](https://github.com/decolua/9router/pull/3433#issuecomment-5364068109)
my branch covered on theirs.

---

## What I work on

Node and TypeScript. Streaming HTTP and server-sent events. Compatibility between the OpenAI,
Claude, and Gemini request shapes — where they agree on paper and diverge in practice. Access
control and SSRF review. Internationalisation, which turns out to be a correctness problem
more often than a translation one.

---

## Contact

[ntdat812.dev@gmail.com](mailto:ntdat812.dev@gmail.com)
