# Nguyen Thanh Dat

Software engineer in Thanh Hoa, Vietnam. I work on AI gateways and LLM tooling — tracing
correctness and security bugs in OpenAI-compatible proxies, then sending the fix upstream.

**English** · [Tiếng Việt](README_vn.md)

---

## Open-source contributions

Six pull requests to two AI gateway projects. Five of them close an issue someone else
reported. Each one carries a regression test that fails before the change and passes after.

| Project | Pull request | What it fixes |
| --- | --- | --- |
| [OmniRoute](https://github.com/diegosouzapw/OmniRoute) | [#10843](https://github.com/diegosouzapw/OmniRoute/pull/10843) `fix(security)` | The outbound guard recognised cloud-metadata hosts by their dotted-decimal spelling, so `http://[::ffff:169.254.169.254]/` walked through a check the docs call absolute. The verdict now follows the address, not the spelling. |
| [OmniRoute](https://github.com/diegosouzapw/OmniRoute) | [#10860](https://github.com/diegosouzapw/OmniRoute/pull/10860) `fix(mcp)` | One hardcoded fetch budget covered every internal server-to-server hop, so provider-bound tool calls inherited a timeout meant for something else. Fixes [#9717](https://github.com/diegosouzapw/OmniRoute/issues/9717). |
| [OmniRoute](https://github.com/diegosouzapw/OmniRoute) | [#10858](https://github.com/diegosouzapw/OmniRoute/pull/10858) `fix(context)` | Base64 documents were measured character by character, so a 1 MB PDF estimated at 350,022 tokens and the request was rejected before it left. Fixes [#10840](https://github.com/diegosouzapw/OmniRoute/issues/10840). |
| [OmniRoute](https://github.com/diegosouzapw/OmniRoute) | [#10857](https://github.com/diegosouzapw/OmniRoute/pull/10857) `fix(catalog)` | With auto routing off, `/v1/models` still advertised every `auto/*` id that the router would reject at request time. Fixes [#10831](https://github.com/diegosouzapw/OmniRoute/issues/10831). |
| [OmniRoute](https://github.com/diegosouzapw/OmniRoute) | [#10853](https://github.com/diegosouzapw/OmniRoute/pull/10853) `fix(i18n)` | Eight locales translated the *status* "Disabled" with the noun for a person who has a disability. The reported issue named only Japanese; the audit found 24 strings. Fixes [#10812](https://github.com/diegosouzapw/OmniRoute/issues/10812). |
| [9router](https://github.com/decolua/9router) | [#3434](https://github.com/decolua/9router/pull/3434) `fix(responses)` | `/v1/responses` never emitted `usage`, so clients read all-zero token counts. Two causes: the usage chunk carries an empty `choices` array, and it arrives after `finish_reason`. Fixes [#3432](https://github.com/decolua/9router/issues/3432). |

+937 / −35 across 28 files. All six are open and awaiting maintainer review.

---

## Security research

I reported four vulnerabilities to the OmniRoute maintainers through private advisories, which
is what the project's `SECURITY.md` asks for.

One is public, because its fix is already on the table as [#10843](https://github.com/diegosouzapw/OmniRoute/pull/10843):
a server-side request forgery (CWE-918) in the outbound host guard. `isCloudMetadataHost()`
compared spellings rather than addresses, so an IPv4-mapped IPv6 literal reached the cloud
metadata endpoint through a block documented as unconditional. The patch normalises the host
before the decision and ships with the bypass payload as a regression test.

The other three are still in triage and unpatched, so no details here. They stay private until
the maintainers have shipped a fix.

---

## What I work on

Node and TypeScript. Streaming HTTP and server-sent events. Compatibility between the OpenAI,
Claude, and Gemini request shapes — where they agree on paper and diverge in practice. Access
control and SSRF review. Internationalisation, which turns out to be a correctness problem more
often than a translation one.

Most of what I find comes from reading an issue nobody has picked up, reproducing it, and
following it down to the line that is actually wrong.

---

## Contact

[ntdat812.dev@gmail.com](mailto:ntdat812.dev@gmail.com)
