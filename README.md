# Nguyen Thanh Dat

Software engineer in Thanh Hoa, Vietnam. I work on AI gateways and agent tooling — the layer
between a coding agent and three hundred model providers, where one correctness bug becomes
every user's bug. I read the issue nobody has picked up, reproduce it, and follow it to the
line that is actually wrong.

Seventeen pull requests merged into [OmniRoute](https://github.com/diegosouzapw/OmniRoute) and
[OpenViking](https://github.com/volcengine/OpenViking), sixteen more in review across
[OpenClaw](https://github.com/openclaw/openclaw), [ECC](https://github.com/affaan-m/ECC),
[ComfyUI](https://github.com/Comfy-Org/ComfyUI) and OpenViking, and five vulnerabilities
reported through private security advisories, every one of which is now fixed.

**English** · [Tiếng Việt](README_vn.md)

---

## Merged

Seventeen merged — sixteen into OmniRoute, an MIT AI gateway fronting 340 providers, and one
into OpenViking. Nine close an issue someone else reported. Each carries a regression test that
fails on the base branch and passes with the change.
[Full list](https://github.com/search?q=author%3Antdat812+is%3Apr+is%3Amerged&type=pullrequests).

Ten that show the range:

| Pull request | What it fixes |
| --- | --- |
| [#10843](https://github.com/diegosouzapw/OmniRoute/pull/10843) `fix(security)` | An SSRF guard that matched cloud-metadata hosts by spelling instead of by address. Detailed below. |
| [#11328](https://github.com/diegosouzapw/OmniRoute/pull/11328) `fix(security)` | The canonical denylist of headers never forwarded upstream was missing two of the RFC 7230 hop-by-hop names, so `proxy-authorization` and `proxy-authenticate` went to the provider. |
| [#11319](https://github.com/diegosouzapw/OmniRoute/pull/11319) `fix(db)` | The proxy-URL validator refused private and cloud-metadata targets using its own dotted-quad regexes, so the same address in another spelling walked straight through. |
| [#11311](https://github.com/diegosouzapw/OmniRoute/pull/11311) `fix(db)` | An operator's group pattern was compiled into a `RegExp` with only `*` substituted, so a metacharacter in the pattern changed what it matched. |
| [#10935](https://github.com/diegosouzapw/OmniRoute/pull/10935) `fix(relay)` | An earlier fix put the attacker-controlled `x-relay-path` behind a guard in the Deno and Vercel relay workers. The Cloudflare worker still concatenated it, so userinfo in the path re-pointed the request past the private-host check. |
| [#10868](https://github.com/diegosouzapw/OmniRoute/pull/10868) `fix(proxy)` | Every egress probe had moved to an IPv6-first endpoint, so an IPv4-only tunnel had no route to it, hung until the deadline, and a proxy carrying live traffic was reported dead. The strategy was wrong, not the constant. Closes [#9694](https://github.com/diegosouzapw/OmniRoute/issues/9694). |
| [#10862](https://github.com/diegosouzapw/OmniRoute/pull/10862) `fix(providers)` | Model sync did not fail on an upstream 401. It quietly degraded to a cached catalog, so a provider with dead credentials still looked healthy. Closes [#9683](https://github.com/diegosouzapw/OmniRoute/issues/9683). |
| [#10858](https://github.com/diegosouzapw/OmniRoute/pull/10858) `fix(context)` | Base64 documents were measured character by character, so a 1 MB PDF estimated at 350,022 tokens and the request was rejected before it ever left. Closes [#10840](https://github.com/diegosouzapw/OmniRoute/issues/10840). |
| [#10853](https://github.com/diegosouzapw/OmniRoute/pull/10853) `fix(i18n)` | Eight locales rendered the *status* "Disabled" as the noun for a person who has a disability. Closes [#10812](https://github.com/diegosouzapw/OmniRoute/issues/10812). |
| [OpenViking #4228](https://github.com/volcengine/OpenViking/pull/4228) `fix(ov_dream)` | A session message whose content was a plain string rather than a block list was not accepted. Closes [#4221](https://github.com/volcengine/OpenViking/issues/4221). |

---

## In review

Sixteen open: five on [ComfyUI](https://github.com/Comfy-Org/ComfyUI), five on
[OpenViking](https://github.com/volcengine/OpenViking), five on
[ECC](https://github.com/affaan-m/ECC), one on [OpenClaw](https://github.com/openclaw/openclaw).
[Full list](https://github.com/search?q=author%3Antdat812+is%3Apr+is%3Aopen&type=pullrequests).

| Pull request | What it fixes |
| --- | --- |
| [OpenClaw #127135](https://github.com/openclaw/openclaw/pull/127135) | Every request to an Alibaba Model Studio provider sent the output-token cap as `max_completion_tokens` — a field the vendor's own OpenAI-compatibility reference does not list. Closes [#127119](https://github.com/openclaw/openclaw/issues/127119). |
| [ComfyUI #15841](https://github.com/Comfy-Org/ComfyUI/pull/15841) | A YAML list in `extra_model_paths.yaml` crashed the loader instead of being read as a list of paths. |
| [ComfyUI #15783](https://github.com/Comfy-Org/ComfyUI/pull/15783) | A model directory that links back to one of its own ancestors makes the walk re-enter the same tree at every level, so one model is listed over and over. Following links is deliberate; detecting the loop was missing. |
| [OpenViking #4233](https://github.com/volcengine/OpenViking/pull/4233) | The memory plugin's URI guard read file *content* as if it were a path. Closes [#4188](https://github.com/volcengine/OpenViking/issues/4188). |
| [OpenViking #4229](https://github.com/volcengine/OpenViking/pull/4229) | A stale PID lock was honoured on macOS without checking that the process holding it was the one it claimed to be. Closes [#4210](https://github.com/volcengine/OpenViking/issues/4210). |
| [ECC #2846](https://github.com/affaan-m/ECC/pull/2846) | The dev-server block decided the script name from raw text rather than from tokens. |
| [ECC #2837](https://github.com/affaan-m/ECC/pull/2837) | The guard blocking `--no-verify` compared the flag against that exact spelling. Git resolves any unambiguous prefix of a long option, so `--no-ver` skipped the hooks. |

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

**And the same shape keeps coming back.** A check that compares how something is *spelled*
against a list, when what decides the outcome is what it *is*. Since that patch I have found it
in a proxy-URL validator ([#11319](https://github.com/diegosouzapw/OmniRoute/pull/11319)), a
group pattern compiled into a `RegExp` without escaping
([#11311](https://github.com/diegosouzapw/OmniRoute/pull/11311)), a `--no-verify` block that
missed git's abbreviated long options ([ECC #2837](https://github.com/affaan-m/ECC/pull/2837)),
a destructive-command classifier hidden by a `sudo` prefix
([ECC #2832](https://github.com/affaan-m/ECC/pull/2832)), and a dev-server block reading raw
text where it should read tokens ([ECC #2846](https://github.com/affaan-m/ECC/pull/2846)). The
fix is the same sentence every time: decide on identity, not on spelling.

---

## Security research

Five vulnerabilities reported to the OmniRoute maintainers through private advisories, the
disclosure route the project's `SECURITY.md` asks for. All five are fixed.

Two by patches I sent: the cloud-metadata bypass described above, and the relay-path gap that
[#10935](https://github.com/diegosouzapw/OmniRoute/pull/10935) closed in the Cloudflare worker.

The maintainers fixed the other three themselves and named the advisory in the code that does
it — `GHSA-mghq-58h3-qcqj` and `GHSA-v7g9-7f55-5g46` on the always-protected route list in
`src/server/authz/routeGuard.ts`, `GHSA-wgwc-crjm-pmwv` on the loopback-only entry beside it.
One of those was a follow-up report: the first fix had left two sibling routes behind, and the
guard now covers them too.

None of the five was published as an advisory, so the patched code is the only public record of
them. Grep the repository for the ids.

---

## How I work

Three things I would rather be judged on than a language list. Each links to the artefact.

**I report what the tests actually said.** I could not get a green build on my Windows box for
[#10843](https://github.com/diegosouzapw/OmniRoute/pull/10843#issuecomment-5355999035), so
instead of ticking the box I showed the identical failure on a clean checkout of the base
branch, traced it to an optional native dependency, and said plainly that a real CI run would
be more authoritative than mine.

**I widen a report when the bug is wider.**
[#10812](https://github.com/diegosouzapw/OmniRoute/issues/10812) reported one bad Japanese
string. The same mistranslation was in eight locales and 24 strings, so
[#10853](https://github.com/diegosouzapw/OmniRoute/pull/10853) fixed all of them and added
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
