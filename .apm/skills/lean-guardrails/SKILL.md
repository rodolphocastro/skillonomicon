---
name: lean-guardrails
description: Use when writing / generating code.
---

# Lean Guardrails

Work on code locally in a safe and controlled way but without overengineering the workflow.

Project-specific guardrails or guidance (be it human or agent focused) should take precedence over the conventions outlined in this skill. 

## Git Conventions

> Keep in mind that project guidelines may override these conventions, if they are present.

- Use "GitHub" flow
  - A single long-lived branch (usually `master` or `main`)
  - Each feature / bugfix / refactor session should have its own branch
  - Once completed the branch gets merged back into the long-lived one
- Use conventional commits
  - Do not over-do descriptions, make them simple
  - Add the co-author information when AI-assisted
- Only humans may `push` to remote or `merge` changes
  - You may provide instructions on how to perform those operations
- Commit atomically, bundling files / changes that need to be kept together but not creating gigantic commits
- You may commit on your own behalf

## Secure development practices

- Never add sensitive information directly to the code base
  - e.g. never add hardcoded passwords, API Keys, connection strings, etc.
- If possible prefer using environment variables to store sensitive information
- Make sure sensitive files are not committed to the repository
  - e.g. `.env` files that aren't examples; files that may contain API or certificate keys


## Code Checks

Checks must be executed before any changes are done, to define the baseline, and then executed before changes get committed.

Precedence of checks are:

1. Dependencies
2. Build / Compile checks
3. Style Checks
4. Unit tests
5. Integration tests
6. Manual tests, by the user

> Important: Those are only for local development, they may serve as a base for a CI pipeline but might not be enough for production environments or bigger teams.

If a step fails it needs to be addressed before proceeding to the next step, either by you or the user, unless they were deferred as part of a baseline.

A failure is defined by a check being "red".

> In case there are errors in the baseline you may prompt the user if they want it to be addressed as part of the session or by themselves.

### Check for dependencies

Step-by-Step:

1. Check if the project has a dependency / package manager defined (such as NPM or UV)
2. If there is no dependency / package manager defined:
   1. Ask the user if they want to define one, provide suggestions based on the project's technology
   2. Adjust documentation to reflect the new dependency / package manager
   3. Commit those changes together
3. Ensure that the package manager is available for you to use, if it isn't this check is red
4. Ensure dependencies can be restored.  If dependencies restore successfully this check is green. If not, this check is red.

> If the technology doesn't have a package manager, dependency manager this check can be skipped.

### Check if quality tools are defined

If the project doesn't have tools for quality checking defined (or available for you to use) you need to make sure they are defined and available before proceeding into the next steps, as far as suitable tools exist for the technology / framework.

1. Check if the project has existing tools for quality checking
2. If there are defined tools: 
   1. Ensure they are available for you to use. 
   2. If they are not available: this check is red. Otherwise, this check is green.
3. Based on the project's languages and frameworks briefly look up what are the "community" default tools
4. Provide suggestions and question the user for:
   1. Which `linting` tools would they like to use?
   2. Which `formatting` tools would they like to use?
   3. Which `testing` (unit and integration) tools would they like to use?
5. Adjust documentation to reflect the new tools
6. Commit the tools to the applicable package / dependency manager, this check is now green.

> Never assume default tools are the go-to, let the human decide based on their own judgment and your curated suggestions.

> Technologies / Projects that have no suitable code quality tools may skip this check. 

### Check that the code builds / compiles

Run the appropriate build (or equivalent) tool for the project's language and framework. Check the result and decide:

1. No errors / no warnings: this check is green.
2. No errors / warnings: this check is yellow.
3. Errors: this check is red.

> User gets to decide on addressing, or not, yellow checks. Build errors always must be addressed before moving on to next checks.

> If the technology doesn't have a builder, compiler or transpiler this check can be skipped.

### Check for code styles

Run the appropriate linting and formatting tools for the project's language and framework. Check the result and decide:

1. No errors and no warnings: this check is green.
2. No errors, but warnings: this check is yellow.
3. Errors: this check is red.

Before actually declaring the status check if the tools offer any type of "fix" flag - if they do you may run the `fix` command and re-run the check.

> User gets to decide on addressing, or not, yellow checks. Errors always must be addressed before moving on to next checks.

### Check the automated test suite

Run the appropriate commands for `unit` and `integration` tests. If any test fail the check is red, otherwise it is green.

### Manual Tests

> This may be skipped when setting the baseline.

You should spin up the project locally, when possible, and guide the user on how to access it.

Then prompt the user to perform manual tests, if applicable, and take their input / feedback.

If they accept what has been done, this check is green. Otherwise, this check is red.
