# Media Advisor Expert System — Conflict Resolution

[![Notebook](https://img.shields.io/badge/interface-Jupyter-F37626?logo=jupyter&logoColor=white)](https://jupyter.org/)
[![Topic](https://img.shields.io/badge/topic-expert%20systems-6E40C9)](#conflict-resolution-policy)
[![Status](https://img.shields.io/badge/status-coursework-blue)](#limitations)

> A rule-based reasoning study that designs a transparent conflict-resolution policy for a media-advisor expert system.

## Project Overview

A media-advisor system may receive several rules about the same item: one source may be historically reliable, another item may be more recent, and a safety or privacy rule may impose a hard constraint. This notebook models that situation and proposes an ordered policy for deciding which rule should win when recommendations conflict.

The goal is not to train a statistical model. It is to make rule priorities explicit, auditable, and easier to test.

## Conflict-Resolution Policy

The proposed policy applies the following principles:

| Priority | Principle | Purpose |
|---:|---|---|
| 1 | **Safety, ethics, and privacy** | Prevent harmful or disallowed recommendations from being promoted by engagement or recency. |
| 2 | **Specificity** | Prefer a rule written for the exact context over a broad default rule. |
| 3 | **Source reliability** | Prefer evidence from a source with stronger historical credibility. |
| 4 | **Recency** | Prefer newer information when the competing rules are otherwise comparable. |
| 5 | **General priority** | Use an explicit rule priority as the final deterministic tie-breaker. |

This ordering separates **hard constraints** from **ranking preferences**. For example, a privacy violation should not be overridden by a high-engagement or breaking-news rule.

## Example Decision Flow

```text
Collect applicable rules
        ↓
Reject actions that violate safety, ethics, or privacy
        ↓
Prefer the most specific applicable rule
        ↓
Compare source reliability and evidence freshness
        ↓
Apply explicit priority and record an explanation
        ↓
Return recommendation + winning rule + rejected conflicts
```

## Example Conflict

Suppose a story is highly engaging but contains an unverified private claim. A recency rule and an engagement rule may recommend immediate publication, while a privacy rule blocks publication. Under this policy, privacy wins because it is a hard constraint. The system should return both the decision and the explanation so a reviewer can audit the result.

## Notebook

The notebook `Expert_System_HomeWork02_.ipynb` contains the coursework analysis and proposed mechanism. Run it from top to bottom after installing Jupyter:

```bash
git clone https://github.com/Ahmedosrf/Expert_System_HomeWork02.git
cd Expert_System_HomeWork02
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\\Scripts\\activate
pip install jupyter
jupyter notebook Expert_System_HomeWork02_.ipynb
```

## Design Principles

- **Deterministic:** Equal inputs should produce the same decision.
- **Explainable:** The system should expose the winning rule and the reason it defeated alternatives.
- **Conservative:** Safety, ethics, and privacy constraints should be fail-safe.
- **Extensible:** New rules should declare scope, priority, evidence requirements, and conflict behavior.
- **Testable:** Each conflict scenario should have an expected winner and explanation.

## Limitations and Next Steps

This repository documents a policy and coursework scenario rather than a complete production inference engine. A stronger implementation would represent rules as structured objects, add a conflict graph, define tie-breaking formally, create unit tests for contradictory rules, and log provenance for every recommendation. Any real media workflow also requires editorial review and clear accountability.

## Maintainer

[Ahmed Osrof](https://github.com/Ahmedosrf)
