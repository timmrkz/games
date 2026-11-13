# CLAUDE.md

Guidance for Claude when working on this repository. Read it fully before
the first change of a session.

## The project

A catalog of games for teaching Brazilian jiu-jitsu: each game with its
setup, goal, win condition, variations and labels, filtered by a chain of
labels, and printed as cards. It is served at the domain in `CNAME`.

Today it is one page, `index.html`, with the published catalog in
`games.json`. It is meant to become a server and a client: the server
owns every rule and all the state, later with a database, and the client
only renders what it sends. See [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

## The skills

How we work is in the skills in `.claude/skills/`. They are written for
any project, so their rules name no file or part of ours, and only some
of their examples come from this catalog. Read the one for the work
before starting it.

| Skill | For |
| --- | --- |
| [architecture](.claude/skills/architecture/SKILL.md) | where a change goes, the server and client split, changing the design |
| [engineering](.claude/skills/engineering/SKILL.md) | how any change is made, root causes, build, CI, speed, docs, measurements |
| [testing](.claude/skills/testing/SKILL.md) | how every test is written, fixed and reviewed, and how a bug is found |
| [pull-requests](.claude/skills/pull-requests/SKILL.md) | how a pull request is opened, sized, described, watched and made ready |
| [review](.claude/skills/review/SKILL.md) | how a pull request is reviewed by an agent that did none of the work |
| [interface](.claude/skills/interface/SKILL.md) | what goes on screen, how actions are answered, measuring and proving a visual fix |
| [collaboration](.claude/skills/collaboration/SKILL.md) | how replies are written, how issues are labelled and picked, writing rules |

Where a skill meets this repository:

| The skills say | Here it is |
| --- | --- |
| the user | the owner of this repository, who asks for the work in the thread |
| the people using the product | coaches and training partners who pick and run games on the mat |
| the product's goals | The project above, and the issue the pull request names |
| the architecture as the project writes it | [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md), and its records in [docs/decisions/](docs/decisions/README.md) |
| the place the project names for a review | a file in `/mnt/project-files/reviews/`, named after the pull request, never committed and never posted on GitHub |
| logged | a GitHub issue, written the way [.github/ISSUE_TEMPLATE/issue.md](.github/ISSUE_TEMPLATE/issue.md) lays it out |
| how a pull request is described | [.github/pull_request_template.md](.github/pull_request_template.md) |

## How the user works

- Changes reach the user as pull requests on GitHub. Never hand them zip
  files or ask them to copy files around.
- They test the page themselves. Say exactly what to open, what to do
  and what should happen.
- Reply in the language of their message.
- Measurements are metric.
