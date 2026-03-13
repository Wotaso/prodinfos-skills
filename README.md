# AnalyticsCLI Skills

Installable Agent Skills for tenant developers integrating AnalyticsCLI analytics.

## Install

Install the analytics/query skill:

```bash
npx skills add wotaso/analyticscli-skills --skill analyticscli-cli
```

Install the TypeScript SDK integration skill:

```bash
npx skills add wotaso/analyticscli-skills --skill analyticscli-ts-sdk
```

If you want both, run both commands.

For OpenClaw via ClawHub:

```bash
npx -y clawhub install analyticscli-cli
npx -y clawhub install analyticscli-ts-sdk
```

## Included Skills

| Skill | Use it for | Target package |
| --- | --- | --- |
| `analyticscli-cli` | Query analytics, validate instrumentation, export bounded data | `analyticscli` `^0.1.0` |
| `analyticscli-ts-sdk` | Integrate or upgrade the JS/TS SDK in web, React Native, or Expo apps | `react-native-analyticscli` `>=0.1.0-preview.0 <0.2.0` |

## Versioning Policy

- `metadata.version` inside a `SKILL.md` is the version of the skill instructions.
- The supported product version range belongs in the body and metadata for the target package, not in the skill name by default.
- The unversioned skill names track the current stable line.
- If a future major release needs materially different instructions, add a sibling skill such as `analyticscli-ts-sdk-v1` instead of mixing incompatible majors into one file.
- Use Git tags and release notes on this public repo for distribution history.

## OpenClaw

The same skill folders work for OpenClaw because they follow the Agent Skills `SKILL.md` format.
You do not need a second skill format for OpenClaw.

If you also want store-style discovery or versioned bundle distribution inside the OpenClaw ecosystem, publish the same folders to ClawHub as an additional channel.

Sync all skills to ClawHub from the monorepo:

```bash
pnpm skills:sync:clawhub -- --all --tags latest
```

## Support Model

- Open issues in the public repo for install problems or unclear instructions.
- Product code and source content are maintained in the AnalyticsCLI monorepo and synced to this public package.
- Mirror sync is triggered from private monorepo changes under `skills/**`.
- Mirror source prefix for this public repo: `skills/`.
