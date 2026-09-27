---
name: jarves-symfony-debugging
description: "Use when diagnosing Symfony/Jarves boot failures, config issues, dependency mismatches, database or mailer setup problems, or environment-specific runtime regressions in this repository. Prefer this skill for configuration-level root-cause analysis and minimal safe fixes."
---

# Jarves Symfony Debugging

Use this skill when the issue is in a Symfony + Jarves project and the user needs a repeatable, low-risk debugging flow.

## When to use this skill

Use it for:
- Symfony app boot or runtime failures
- Broken configuration in `app/config/`
- Propel/database connection problems
- Composer dependency or version conflicts
- Mailer, service, or parameter resolution issues
- Jarves installation and setup tasks
- Environment-specific regressions that affect local or CI behavior

Do not use it for unrelated frameworks, general-purpose coding tasks, or broad refactors outside this project.

## Goal

Diagnose the underlying root cause before making a change, then apply the smallest fix that restores the intended behavior and validate it with the most relevant project command.

## Workflow

### 1. Define the failure clearly

Start by identifying:
- the exact symptom
- which command or entrypoint failed
- the environment involved
- whether the issue is configuration, dependency, or runtime logic

Examples:
- `php bin/console` fails during bootstrap
- app loads but database connections fail
- mailer or service wiring is broken
- a config value is invalid or environment-specific

### 2. Read the relevant project files first

Inspect the smallest relevant scope before editing.

Likely files:
- `app/config/*.yml`
- `app/AppKernel.php`
- `composer.json`
- `bin/console`
- relevant service or bundle code in `src/`

Focus on the exact config path implicated by the error. Do not scan the whole repo unless needed.

### 3. Trace the data flow to the root cause

Ask:
- What setting is being resolved?
- What parameter or environment value is expected?
- Is the app expecting a value that is missing, stale, or incompatible?
- Did a recent config change affect service registration, database DSN, or parameters?

Common root causes:
- placeholders unresolved in YAML
- database credentials mismatched
- outdated bundle/config syntax for the Symfony version
- dependency mismatch between installed Composer packages and app expectations
- missing environment-specific values

### 4. Form a single hypothesis

Before editing, state the likely root cause in one sentence.

For example:
- "The app is failing because the database DSN references a parameter that is not defined in the active environment."
- "The service is misconfigured because the YAML uses an outdated mailer structure for this Symfony version."

Only proceed with a fix if the hypothesis matches the observed error and the code path.

### 5. Make the smallest safe change

Preferred change pattern:
- edit one precise config block or parameter
- preserve existing environment parameterization
- avoid speculative refactors
- keep changes reversible and localized

Prefer:
- `%database_driver%`, `%database_host%`, `%database_name%` style placeholders
- project conventions already used elsewhere in the stack
- minimal, explicit changes aligned with Symfony 3-era configuration patterns

Avoid:
- broad YAML rewrites
- changing unrelated services
- hardcoding local values into shared configuration without user approval

### 6. Validate with the smallest relevant command

After the fix, validate with the most direct command that checks the affected behavior.

Examples:
- `php bin/console cache:clear`
- `php bin/console lint:yaml app/config`
- `php bin/console about`
- `composer validate`
- a focused Symfony console command tied to the failing feature

Do not claim the fix works without a fresh validation result.

## Completion checklist

The work is complete only when all of the following are true:
- the root cause has been identified and explained
- the fix is minimal and matches the actual failure
- the relevant validation command was run successfully
- the user understands what changed and why

## Example prompts

- "The Symfony app won’t boot after the config change. Help me diagnose it."
- "Why is the database connection failing in this Jarves project?"
- "Review the mailer or Propel configuration and identify the exact breakage."
- "I updated the YAML, but the app is still failing. What should I check next?"
- "What command should I run to validate the fix after a Symfony config edit?"

## Output style

Respond with:
- a short root-cause summary
- the exact files or config blocks involved
- the minimal fix
- the validation command and outcome
- any remaining assumptions or follow-up checks

Keep the response concise and practical.
