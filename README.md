# ErinOS Skills

Skill catalog for [ErinOS](https://github.com/erinos-ai/erinos-core). Each skill teaches Erin how to use a third-party service by providing setup instructions and command references.


## Included Providers

| Provider | Skills |
|----------|--------|
| **Google Workspace** | Calendar, Gmail, Drive, Sheets, Docs, Slides, People, Tasks |
| **Spotify** | Playback |
| **Hue** | Lights |
| **Sonos** | Speakers |
| **Home Assistant** | Control |


## Structure

Skills are organized by provider. Each provider has a `provider.yml` and one or more skill directories with a `SKILL.md`:

```
google/
  provider.yml           Auth type, env mappings
  shared/SKILL.md        Shared docs (prepended to all Google skills)
  calendar/SKILL.md      Google Calendar skill
  gmail/SKILL.md         Gmail skill
  ...
spotify/
  provider.yml
  playback/SKILL.md
```

### provider.yml

Defines how the provider authenticates and what environment variables to inject:

```yaml
auth:
  type: oauth          # or "local"
env:
  GOOGLE_WORKSPACE_CLI_TOKEN: access_token
```

### SKILL.md

YAML frontmatter with a name and description, followed by markdown documentation:

```markdown
---
name: gws-calendar
description: "Google Calendar: Manage calendars and events."
---

# calendar (v3)

## Commands
...
```

The `shared/` directory (if present) contains documentation that gets prepended to all skills for that provider.


## Installing Skills

Skills are installed to `~/.erinos/skills/` on the appliance. You can install from the registry:

```ruby
SkillManager.new.install("google")
```

Or copy a skill directory manually:

```bash
scp -r google/ erinos@appliance:~/.erinos/skills/google/
```

The skill registry in erinos-core picks up any directory with a `provider.yml` automatically. Restart the server after adding new skills.


## Creating a New Skill

1. Create a provider directory with `provider.yml`:

```yaml
auth:
  type: local
env:
  MY_SERVICE_TOKEN: api_key
```

For OAuth providers, add the provider config to [erinos-relay](https://github.com/erinos-ai/erinos-relay)'s `providers.yml`.

2. Create a skill directory with `SKILL.md`:

```markdown
---
name: myservice-control
description: Control my service
---

## Setup

How to get credentials.

## Commands

CLI commands or API calls Erin should use.
```

3. Install the skill to the appliance and restart the server.
