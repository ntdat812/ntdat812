# Nguyen Thanh Dat

Software engineer in Thanh Hoa, Vietnam. I work on AI gateways and agent tooling — the layer
between a coding agent and three hundred model providers, where one correctness bug becomes
every user's bug. I read the issue nobody has picked up, reproduce it, and follow it to the
line that is actually wrong.

Nine pull requests merged into [OmniRoute](https://github.com/diegosouzapw/OmniRoute), six more
in review across [OpenViking](https://github.com/volcengine/OpenViking),
[ECC](https://github.com/affaan-m/ECC) and OmniRoute, and four vulnerabilities reported through
private security advisories — two of which now have merged fixes.

**English** · [Tiếng Việt](README_vn.md)

---

## Merged

Nine pull requests merged into OmniRoute, an MIT AI gateway fronting 340 providers. Six of
them close an issue someone else reported. Each carries a regression test that fails on the
base branch and passes with the change.

| Pull request | What it fixes |
| --- | --- |
| [#10843](https://github.com/diegosouzapw/OmniRoute/pull/10843) `fix(security)` | An SSRF guard that matched cloud-metadata hosts by spelling instead of by address. Detailed below. |
| [#10935](https://github.com/diegosouzapw/OmniRoute/pull/10935) `fix(relay)` | An earlier fix replaced concatenation of the attacker-controlled `x-relay-path` with a guard — but only in the Deno and Vercel relay workers. The Cloudflare worker generates the same shape and still concatenated, so userinfo in the path re-pointed the request past the private-host check. |
| [#10941](https://github.com/diegosouzapw/OmniRoute/pull/10941) `fix(relay)` | Puts all three relay workers behind that one shared guard, so the next worker added cannot drift out of step with it. |
| [#10868](https://github.com/diegosouzapw/OmniRoute/pull/10868) `fix(proxy)` | Every egress probe had been moved to an IPv6-first endpoint, so an IPv4-only tunnel had no route to it, hung until the deadline, and a proxy carrying live traffic was reported dead. Swapping the constant back only re-breaks the other case — the strategy was wrong, not the value. Closes [#9694](https://github.com/diegosouzapw/OmniRoute/issues/9694). |
| [#10862](https://github.com/diegosouzapw/OmniRoute/pull/10862) `fix(providers)` | Model sync did not fail on an upstream 401. It quietly degraded to a cached catalog, so a provider with dead credentials still looked healthy. Closes [#9683](https://github.com/diegosouzapw/OmniRoute/issues/9683). |
| [#10860](https://github.com/diegosouzapw/OmniRoute/pull/10860) `fix(mcp)` | One hardcoded fetch budget covered every internal server-to-server hop, so provider-bound tool calls inherited a timeout meant for something else. Closes [#9717](https://github.com/diegosouzapw/OmniRoute/issues/9717). |
| [#10858](https://github.com/diegosouzapw/OmniRoute/pull/10858) `fix(context)` | Base64 documents were measured character by character, so a 1 MB PDF estimated at 350,022 tokens and the request was rejected before it ever left. Closes [#10840](https://github.com/diegosouzapw/OmniRoute/issues/10840). |
| [#10857](https://github.com/diegosouzapw/OmniRoute/pull/10857) `fix(catalog)` | With auto routing off, `/v1/models` still advertised every `auto/*` id that the router would reject at request time. Closes [#10831](https://github.com/diegosouzapw/OmniRoute/issues/10831). |
| [#10853](https://github.com/diegosouzapw/OmniRoute/pull/10853) `fix(i18n)` | Eight locales rendered the *status* "Disabled" as the noun for a person who has a disability. Closes [#10812](https://github.com/diegosouzapw/OmniRoute/issues/10812). |

---

## In review

| Project | Pull request | What it fixes |
| --- | --- | --- |
| [OmniRoute](https://github.com/diegosouzapw/OmniRoute) | [#10958](https://github.com/diegosouzapw/OmniRoute/pull/10958) `fix(desktop)` | GitHub rewrites spaces in an uploaded asset name to `.`; electron-builder writes the same name into `latest.yml` with `-`. NSIS carries the one default artifact name containing spaces, so the manifest and the published asset can never agree and the in-app updater 404s on every release. Fixes [#10947](https://github.com/diegosouzapw/OmniRoute/issues/10947). |
| [OmniRoute](https://github.com/diegosouzapw/OmniRoute) | [#10951](https://github.com/diegosouzapw/OmniRoute/pull/10951) `fix(resilience)` | `least-used` sorts connections by `lastUsedAt` and was the only strategy reading it that never wrote it, so the rotation it promised never happened. Fixes [#10945](https://github.com/diegosouzapw/OmniRoute/issues/10945). |
| [OpenViking](https://github.com/volcengine/OpenViking) | [#4182](https://github.com/volcengine/OpenViking/pull/4182) `fix(observability)` | Three BFF endpoints take a free-form `?timezone=`, and a malformed value returned HTTP 500 rather than falling back to the server default. `ZoneInfo()` rejects a bad key two different ways; only one was handled. |
| [OpenViking](https://github.com/volcengine/OpenViking) | [#4173](https://github.com/volcengine/OpenViking/pull/4173) `fix(observability)` | Request logs stored the route template, so a 404 on a parameterised endpoint recorded `/sessions/{session_id}` and the failing id was unrecoverable. The raw path was already in the payload and simply dropped. |
| [ECC](https://github.com/affaan-m/ECC) | [#2832](https://github.com/affaan-m/ECC/pull/2832) `fix(gateguard)` | The destructive-command classifier keys on the first token, so `sudo`, `doas`, and `VAR=value` prefixes hid the command being judged. |
| [ECC](https://github.com/affaan-m/ECC) | [#2829](https://github.com/affaan-m/ECC/pull/2829) `fix(gateguard)` | One trailing `\b` was shared across every arm of a destructive-SQL alternation, so the guard's reach did not match its intent. |

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

Four vulnerabilities reported to the OmniRoute maintainers through private advisories, which
is the disclosure route the project's `SECURITY.md` asks for.

Two now have merged fixes: the cloud-metadata bypass described above, and the relay-path gap
that [#10935](https://github.com/diegosouzapw/OmniRoute/pull/10935) closed in the Cloudflare
worker. Both are public only because their patches are public.

The other two are still in triage and unpatched, so there are no details here — not the
component, not the class. They stay private until the maintainers ship a fix. One of them is a
good deal more serious than either of the two above.

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

Node, TypeScript, and Python. Streaming HTTP and server-sent events. Compatibility between the
OpenAI, Claude, and Gemini request shapes — where they agree on paper and diverge in practice.
Access control, SSRF review, and command-classification guards. Internationalisation, which
turns out to be a correctness problem more often than a translation one.

---

## Contact

[ntdat812.dev@gmail.com](mailto:ntdat812.dev@gmail.com)
