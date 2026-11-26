# Contributing to Skillonomicon

## Contribution Guidelines

First and foremost: Be nice to other contributors.

Use GitHub Flow, branching out of `main` and returning to it through a Pull Request and a `squash merge`.

Commit Messages should follow the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) specification.

You are responsible for the quality of your contributions, be it handcrafted or ai-assisted. If you're using AI assistance for most of your contribution please call it out on the pull request.

## Runbooks

Official docs are available under [apm's own website](https://microsoft.github.io/apm/producer/write-primitives/#skills).

### Creating new Skills

1. Scaffold a new folder under `.apm/skills` with the name of the new skill
2. Create a SKILL.md file in that folder
3. Ensure the frontmatter is present on the new file
4. Make sure the skill's name, in the frontmatter, matches the folder name
5. Add a link to the skill's page in the [README.md](README.md) file.

> If the skill contains many directives consider splitting it up between a SKILL.MD + `references`, `assets` and `scripts` folders.

### Publishing Versions

1. Bump the version under [apm.yml](apm.yml) accordingly:
   1. Major bump for breaking changes
   2. Minor bump for new features
   3. Hotfix bump for hotfixes
2. Commit the bump on its own commit (usually with a `chore` convention)
3. Merge your changes through a Pull Request
4. Create a new tag `vx.y.z` where `x.y.z` matches the value defined on `apm.yml` on that merge commit.
5. Create a new release on GitHub with the tag and the new version as title.

> Optionally the tag and the version may be both done through GitHub's web ui.