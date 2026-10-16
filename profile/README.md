# greeterHQ

**The world's most tested "Hello World".**

Generative AI has made "good enough" code almost free. greeterHQ is a public
experiment to see whether the time saved can be reinvested in practices that
promise high quality and low maintenance, but are often too costly to follow:

1. **Hexagonal Architecture**: one dependency-free core, many adapters (CLI, HTTP, ...)
2. **Full-Stack Testing**: multiple facets of quality checked before every deployment
3. **Live Documentation**: specs as the single source of truth, evolving with the code

**Spec-driven development** (OpenSpec) steers the AI agents and stores the live documentation.

📖 Read the introduction: [Why I'm Building the World's Most Tested "Hello World"](LINK)

## Quick start

```sh
# CORE
gem install greeter-core
# CLI
greeter greet --name Ada
# HTTP
curl -X POST https://API_URL/greetings \
  -H "Idempotency-Key: $(uuidgen)" -d '{"name":"Ada"}'
```

## Architecture
```mermaid
flowchart TD
    subgraph core["greeter-core (gem)"]
        D["Domain: GreetingService, GuestName<br/>Ports: Clock, GreetingPresenter"]
    end

    subgraph cli["greeter-cli"]
        C["Command-line client"]
    end

    subgraph http["greeter-http"]
        H["Lambda handler → Rack app<br/>application / ports / adapters"]
    end

    AG["API Gateway (HTTP API)"]
    DB[("DynamoDB<br/>per-guest counts + idempotency")]

    C -->|composes| core
    H -->|composes| core
    AG -->|POST /greetings| H
    H -->|GetItem / Put / Update| DB

    classDef gem fill:#eef,stroke:#88a
    class core gem
```

## Repositories
| Repo | What it is | Status |
|--|--|--|
| [greeter-core](https://github.com/greeterhq/greeter-core) | Pure Ruby domain core, no I/O. Published on RubyGems. | ![CI](BADGE) ![Gem](BADGE) |
| [greeter-cli](https://github.com/greeterhq/greeter-cli) | Command-line client composing greeter-core | ![CI](BADGE) |
| [greeter-http](https://github.com/greeterhq/greeter-http) | AWS Lambda + API Gateway + DynamoDB, idempotent per-guest counts | ![CI](BADGE) |

## Testing strategy
| Layer | Tooling | Status |
|--|--|--|
| Unit (domain) | RSpec, in-memory test doubles | ✅ |
| Behaviour (BDD) | Cucumber / Gherkin | ✅ |
| Static analysis | RuboCop | ✅ |
| Cross-adapter compatibility | Shared specs across CLI and HTTP | 🔜 |
| Contract | Pact | 🔜 |
| Security | OWASP ZAP, dependency scanning | 🔜 |
| Performance | k6 against SLOs | 🔜 |
| Observability | OpenTelemetry | 🔜 |

## Live documentation
Specs live in [openspec/](LINK). Flow:
stakeholder + requirements agent → spec → code-generating agents → tests → CI.

## Follow along
Weekly posts on what I built, what broke and what I learned:
[LinkedIn](LINK) · Built by Adam Bonsu, SC-cleared Test Lead.