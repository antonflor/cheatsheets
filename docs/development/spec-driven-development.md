# Spec-Driven Development Cheat Sheet

> **Applies to:** General software-development and AI-assisted coding workflows
> **Last reviewed:** 2026-07-18

Spec-driven development (SDD) is a workflow in which requirements, constraints, acceptance criteria, and implementation plans are written before substantial coding begins.

The specification defines **what** must be built. The plan defines **how** it will be built. Tasks organize the work, and tests provide evidence that the implementation meets the specification.

Related visual: [Spec-driven development lifecycle](../visual-guides.md#spec-driven-development-lifecycle)

> [!CAUTION]
> A detailed specification does not guarantee a correct implementation. Review generated requirements, code, tests, security assumptions, migrations, and deployment procedures before merging or releasing changes.

## Quick definition

```text
Prompt = initial request
Specification = expected behavior and constraints
Plan = technical approach
Tasks = ordered implementation work
Tests = evidence that acceptance criteria are met
Git = change history and review
CI/CD = automated validation and delivery
```

SDD is commonly Git-based, but Git is not the methodology itself. Git records and reviews the change; the specification provides the source of truth for what the change must accomplish.

The usual term is **spec-driven development**. Deployment may be part of the plan, but it is only one stage of the larger development workflow.

## When to use it

Use a complete SDD workflow for changes with meaningful complexity, risk, ambiguity, or handoff requirements:

- new applications or substantial features;
- infrastructure or network automation changes;
- database migrations and API integrations;
- security-sensitive or compliance-sensitive work;
- work delegated to an AI coding agent;
- projects that may move between developers, teams, or tools;
- changes that require a documented rollout or rollback plan.

Use a lighter issue, checklist, or pull-request description for low-risk work such as typo corrections, obvious one-line fixes, formatting changes, and routine dependency updates.

Scale the specification to the risk and complexity of the change. Do not create large documentation packages for trivial work.

## Core artifacts

| Artifact | Primary question | Typical contents |
|---|---|---|
| Project principles | What rules must all work follow? | Security, quality, testing, compatibility, and operational guardrails |
| Specification | What and why? | Problem, users, goals, requirements, constraints, and acceptance criteria |
| Technical plan | How? | Architecture, components, contracts, migrations, tests, deployment, and rollback |
| Task list | In what order? | Small dependency-aware implementation and validation steps |
| Acceptance tests | How is success proven? | Test cases mapped to required behavior |
| Decision record | Why was this tradeoff chosen? | Context, options, decision, consequences, and revisit conditions |

Not every change requires every artifact. The minimum useful chain is:

```text
Requirement -> implementation task -> code change -> validation evidence
```

## Recommended workflow

1. **Define the problem.** Describe who is affected, what currently happens, what should happen instead, and why the change matters.
2. **Write the specification.** Record goals, non-goals, requirements, constraints, edge cases, and measurable acceptance criteria.
3. **Clarify ambiguity.** Resolve undefined terms, permissions, failure behavior, compatibility expectations, and operational requirements before planning.
4. **Create the technical plan.** Choose the architecture, components, interfaces, data changes, tests, rollout, observability, and rollback strategy.
5. **Break the plan into tasks.** Create small, ordered, independently verifiable units of work.
6. **Implement one task at a time.** Inspect the existing system, make the smallest complete change, validate it, and record it in Git.
7. **Test against the specification.** Verify behavior, failure paths, security controls, migrations, and operational requirements.
8. **Review the complete change.** Compare the pull request with the specification rather than reviewing code in isolation.
9. **Update the artifacts when requirements change.** Change the specification or plan before silently changing implementation behavior.

## Clarification checklist

Before creating the technical plan, confirm:

- [ ] The intended user or operator is identified.
- [ ] Goals and non-goals are explicit.
- [ ] Important terms are defined.
- [ ] Permissions and trust boundaries are documented.
- [ ] Failure and retry behavior is described.
- [ ] Compatibility requirements are known.
- [ ] Performance requirements are measurable.
- [ ] Security, privacy, and data-retention requirements are included.
- [ ] Acceptance criteria can be tested.
- [ ] Open questions have owners or documented assumptions.

A useful test is: **Could two developers read the specification and reach approximately the same understanding of the expected result?**

## Git-based workflow

A common repository workflow is:

```text
Approve idea
-> create feature branch
-> commit specification
-> review and refine specification
-> commit plan and tasks
-> implement in focused commits
-> run tests and quality checks
-> open pull request
-> compare code with acceptance criteria
-> merge
-> deploy and verify
```

Recommended branch and commit practices:

- keep the specification and implementation in the same repository;
- use a dedicated branch for the feature or change;
- commit the specification before or with the first implementation work;
- keep implementation commits focused and reviewable;
- reference requirements or task identifiers when useful;
- update the spec, plan, and tasks when the approved scope changes;
- preserve test output, screenshots, plans, or deployment evidence when the risk warrants it.

See [git.md](git.md) for branch, commit, pull-request, history-repair, and recovery commands.

## Minimal repository layout

```text
project/
├── README.md
├── AGENTS.md
├── docs/
│   ├── architecture/
│   └── decisions/
├── specs/
│   └── 001-feature-name/
│       ├── spec.md
│       ├── plan.md
│       ├── tasks.md
│       ├── acceptance.md
│       └── contracts/
├── src/
└── tests/
```

Tool-specific layouts vary. The filenames matter less than keeping requirements, decisions, tasks, code, and validation traceable.

## Minimal specification template

```markdown
# <Feature name>

## Problem

Who is affected, what happens today, and why is a change needed?

## Goals

- <Required outcome>

## Non-goals

- <Explicitly excluded behavior>

## Requirements

- **REQ-001:** The system must <measurable behavior>.

## Acceptance criteria

- **AC-001:** Given <context>, when <action>, then <observable result>.

## Constraints

- <Compatibility, platform, dependency, security, or operational constraint>

## Edge cases

- <Failure, empty state, concurrency, retry, or boundary condition>

## Open questions

- <Question, owner, and decision deadline>
```

Write requirements so that each one describes observable behavior. Replace vague terms such as “fast,” “secure,” and “user-friendly” with measurable or reviewable criteria.

## Minimal technical plan template

```markdown
# <Feature name> implementation plan

## Existing system context

## Proposed design

## Components and files affected

## Data model or state changes

## APIs and contracts

## Security considerations

## Testing strategy

## Observability

## Deployment or migration sequence

## Rollback plan

## Risks and tradeoffs
```

The specification should remain focused on what and why. Put framework, library, schema, component, and deployment decisions in the plan.

## Task-list template

```markdown
# <Feature name> tasks

- [ ] T001 Add or update the required contract.
- [ ] T002 Add the data or state transition.
- [ ] T003 Implement the smallest end-to-end behavior.
- [ ] T004 Add validation and failure handling.
- [ ] T005 Add unit and integration tests.
- [ ] T006 Add logs, metrics, or alerts.
- [ ] T007 Update user and operator documentation.
- [ ] T008 Validate deployment and rollback procedures.
```

Each task should have one clear result, identify likely affected components, and include a validation step. Avoid tasks such as “build the backend” that hide many unrelated changes.

## Acceptance-test pattern

```gherkin
Given <starting state>
When <action occurs>
Then <required observable behavior>
And <additional required behavior>
```

Map tests back to requirement or acceptance-criterion identifiers when the feature is large, regulated, security-sensitive, or likely to be maintained by multiple teams.

## Small worked example

Initial request:

```text
Add a readiness endpoint.
```

Improved specification:

```markdown
## Requirements

- **REQ-001:** The service must expose `GET /health/ready`.
- **REQ-002:** The endpoint must return HTTP 200 only after startup initialization completes and required database connectivity succeeds.
- **REQ-003:** The endpoint must return HTTP 503 when a required dependency is unavailable.
- **REQ-004:** The response must not expose credentials, connection strings, internal hostnames, or stack traces.
- **REQ-005:** The endpoint must respond within 500 milliseconds under normal operating conditions.

## Non-goals

- The endpoint does not report detailed dependency diagnostics to unauthenticated callers.
- The endpoint does not replace metrics, logs, or alerting.

## Acceptance criteria

- **AC-001:** Given a healthy initialized service, when `/health/ready` is requested, then the response is HTTP 200.
- **AC-002:** Given an unavailable required database, when `/health/ready` is requested, then the response is HTTP 503.
- **AC-003:** Given any endpoint state, the response body contains no sensitive connection or host information.
```

Example tasks:

```markdown
- [ ] T001 Identify the existing health-check framework and routing conventions.
- [ ] T002 Add readiness evaluation for initialization and database connectivity.
- [ ] T003 Add the `/health/ready` route and sanitized response.
- [ ] T004 Add healthy, unavailable-dependency, timeout, and sensitive-data tests.
- [ ] T005 Add operator documentation and deployment verification steps.
```

The improved version defines success, failure behavior, response safety, performance, exclusions, and validation evidence before implementation begins.

## Pull-request review checklist

- [ ] Every implemented behavior maps to an approved requirement.
- [ ] Every acceptance criterion has test or review evidence.
- [ ] The implementation does not add undocumented scope.
- [ ] Non-goals remain excluded.
- [ ] Error, timeout, retry, and rollback paths are covered.
- [ ] Security and privacy assumptions were reviewed.
- [ ] Migrations are backward-compatible or have a controlled transition plan.
- [ ] Logs and metrics avoid secrets and sensitive data.
- [ ] Documentation reflects the final implementation.
- [ ] The specification and plan were updated when approved decisions changed.

## Common mistakes

- treating a large natural-language prompt as a complete specification;
- choosing implementation details before defining the problem;
- omitting non-goals and allowing scope to expand silently;
- using requirements that cannot be objectively reviewed or tested;
- generating tasks before clarifying permissions, failure behavior, and constraints;
- allowing an AI agent to change requirements without recording the decision;
- checking off tasks without validation evidence;
- writing tests that only mirror the implementation instead of the required behavior;
- letting the specification become stale after the code changes;
- applying a heavyweight process to trivial low-risk work.

## Tool-assisted SDD

Tools can generate and maintain artifacts, but the underlying workflow is tool-neutral.

- GitHub Spec Kit commonly uses a **spec -> plan -> tasks -> implement** sequence and provides optional clarification, checklist, consistency-analysis, and convergence stages.
- Kiro Specs organizes feature work around requirements, design, tasks, implementation, and pull-request review.
- A manual Markdown workflow can use the same principles without adopting a specific CLI, editor, model, or hosted service.

Treat generated specifications and plans as drafts requiring engineering review. Tool output does not replace product, security, architecture, or operational judgment.

## Official references

- [GitHub Spec Kit documentation](https://github.github.com/spec-kit/)
- [GitHub Spec Kit quick start](https://github.github.com/spec-kit/quickstart.html)
- [GitHub Spec Kit repository](https://github.com/github/spec-kit)
- [Kiro Specs documentation](https://kiro.dev/docs/web/specs/)
- [Git documentation](https://git-scm.com/docs)
