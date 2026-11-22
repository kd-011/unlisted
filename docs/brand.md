---
slug: unlisted-brand
name: unlisted brand
repo: .
status: active
last_verified: 2026-11-22
needs_review: true
---

# unlisted: brand

Who the project speaks as. There is no DESIGN.md (it is a command line tool); this is how it talks. Drafted only from copy already shipped, every line cited; the operator owns it and clears needs_review once checked.

## Name and casing

- The name is `unlisted`, all lowercase, in the README title, the usage text, every error prefix and the binary name. <!-- src: README.md:1, internal/cli/cli.go:35, internal/cli/cli.go:301 -->
- Prose keeps it lowercase, including at the start of a sentence, and sets it in code font where it names the command: "`unlisted` answers one question". <!-- src: docs/alternatives.md:3, SECURITY.md:11 -->
- The GitHub Action is `unlisted-check`, because the Marketplace refuses a name that is also a GitHub account. <!-- src: action.yml:1, CHANGELOG.md:9-11 -->
- The owner handle shipped in the module path, licence and action author is `kd-011`. <!-- src: go.mod:1, LICENSE:3, action.yml:3 -->

## One-liner

"Compares the services you say you run with what your machine exposes through Cloudflare Tunnel, Caddy, nginx, Docker and listening ports, and lists where they disagree." <!-- src: README.md:5 -->

The command's own usage line says it shorter: "diff what you say you run against what is actually exposed." <!-- src: internal/cli/cli.go:35 -->

## Audience

People who run their own services at home or on a small server behind a tunnel or reverse proxy: the README speaks to "your registry", "your Caddyfile and tunnel config", "your machine", and the demo is a fake homelab. <!-- src: README.md:5, README.md:52, docs/action.md:3 -->

## Is and is not

- It is a one-shot, read-only diff between a short YAML list and what the machine exposes. <!-- src: README.md:54, README.md:60 -->
- It is one static Go binary with no network calls unless `--probe` is passed. <!-- src: README.md:54 -->
- It is honest about what it cannot see: it says "no auth detected", never "insecure", and its report says which sources it could not read. <!-- src: README.md:36, README.md:28 -->
- It is not a way to learn what is reachable from the internet; an outside scan answers that. <!-- src: README.md:58 -->
- It is not continuous monitoring, and not for several machines at once. <!-- src: README.md:60-62 -->

## Voice

Plain, dry and specific. Adjectives are a draft and are listed under Unverified.

- Do: say what the tool cannot do before the reader finds out ("When not to use it" is a README section), give a number or a file and line, and name nearby tools without ranking them. <!-- src: README.md:56-64, docs/alternatives.md:3 -->
- Do: write in the second person and the active voice, with short sentences and the actual command. <!-- src: README.md:19-26 -->
- Do not: use emoji or em dashes in code, comments or docs. <!-- src: CONTRIBUTING.md:51 -->
- Do not: claim a result it has not measured. "I have not run them side by side on one machine, so this covers what each is for and says nothing about how they perform." <!-- src: docs/alternatives.md:3 -->
- Banned words: none are listed in the repo; see Unverified.

## Copy bank

- Repo and README title: `# unlisted`, then a one-sentence description, then the demo image, then Install. <!-- src: README.md:1-11 -->
- Section headings are plain nouns or phrases: "Install", "Try it", "What it finds", "When not to use it", "Building". <!-- src: README.md:11, README.md:19, README.md:30, README.md:56, README.md:66 -->
- Clean-run line: "Nothing unlisted." with "Everything exposed is in the registry, and everything in the registry is being served." <!-- src: internal/report/text.go:35-39 -->
- First run with no registry: "No registry.yml here, so this lists everything the machine exposes. Run unlisted init to write a registry from it, then edit the names and auth lines." <!-- src: internal/check/check.go:26 -->
- Error tone: say what is wrong and the next command, lowercase, prefixed `unlisted:`. "registry.yml not found. Run 'unlisted init' to write a starter registry, or pass -f". <!-- src: internal/cli/cli.go:198, internal/cli/cli.go:301 -->
- Finding fix lines are imperatives: "add it to the registry, or remove the tunnel ingress rule". <!-- src: internal/check/check.go:189, docs/demo.txt:12 -->
- Action description: "Compare the services in your registry with what your configs expose, and fail on the gaps." <!-- src: action.yml:2 -->
- Issue template prompts: "The config that was misread", "What happened", "What you expected". <!-- src: .github/ISSUE_TEMPLATE/bug_report.yml:8-22 -->
- OG text: none is set in the repo; the social preview is an image only. <!-- src: .github/social-preview.png -->

## Assets

- Social preview image, 1280 by 640 PNG: `.github/social-preview.png`. <!-- src: .github/social-preview.png, internal/kit/kit_test.go:399-409 -->
- README hero image of the demo output: `docs/demo-hero.png`; full report image: `docs/demo.png`; text copy: `docs/demo.txt`. <!-- src: README.md:7-9, Makefile:28-33 -->
- Action Marketplace icon: Feather `eye-off`, colour `gray-dark`. <!-- src: action.yml:4-6 -->
- No logo or favicon: there is no site, and none is in the repo. <!-- src: README.md, .github/social-preview.png -->

## Contact and legal

- Security and conduct reports go through GitHub private vulnerability reporting ("Report a vulnerability" on the Security tab); the project publishes no email address. <!-- src: SECURITY.md:3-5, CODE_OF_CONDUCT.md:19, .github/ISSUE_TEMPLATE/config.yml:3-5 -->
- Bugs go to the repository's issues, with a sanitised config snippet. <!-- src: CONTRIBUTING.md:5-7 -->
- Licence: MIT, "Copyright (c) 2026 kd-011". <!-- src: LICENSE:1-3 -->
- No privacy policy or terms page: it has no site or accounts. <!-- src: README.md, SECURITY.md:11 -->

## Unverified

- The three voice adjectives (plain, dry, specific) are the drafter's reading of the shipped copy, not a stated brand choice.
- Banned words beyond emoji and em dashes: the repo lists none, so any further list would be a guess.
- "Never `Unlisted`" is inferred: the capitalised form appears nowhere in shipped copy, but nothing states it as a rule.
- The GitHub repository description and topics, which are the closest thing to a meta description, are not in the repo and were not read.
- Who the audience says it is (r/selfhosted) comes from outside the repo; the README only implies a self-hoster.
