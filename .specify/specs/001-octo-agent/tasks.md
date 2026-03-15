# Tasks: 001-octo-agent (Core Infrastructure)

## Setup & Scaffolding
- [ ] Initialize TypeScript project (`package.json`, `tsconfig.json`).
- [ ] Setup `src/` directory structure: `core/`, `agents/`, `guardrails/`, `observability/`.
- [ ] Create `README.md` introducing the "Octo-Agentic" framework.

## Core Nervous System (Blackboard)
- [ ] Implement `src/core/Blackboard.ts`: Define `Event` interface (`id`, `timestamp`, `agentId`, `eventType`, `payload`, `status`). Create `publish`, `subscribe`, and `getHistory` methods.
- [ ] Implement `src/observability/AuditLogger.ts`: Subscribe to all `Blackboard` events. Format and write to an append-only log (e.g., `audit.log`).
- [ ] Create `src/observability/EventReplayer.ts` for root-cause analysis from logs.

## Digital Instincts (Guardrails)
- [ ] Implement `src/guardrails/GuardrailPerimeter.ts`: Middleware class that intercepts actions proposed to the Blackboard.
- [ ] Create `src/guardrails/validators/BudgetValidator.ts` (e.g., max $1000).
- [ ] Create `src/guardrails/validators/EndpointAllowlist.ts` (e.g., only allow `api.company.internal`).
- [ ] Create `src/guardrails/validators/EthicsFilter.ts` (mock).

## The Cephalopod (Agents)
- [ ] Define abstract base class `src/agents/AutonomousArm.ts`: Needs a `subscribe` pattern matching specific task types on the Blackboard. Needs an `execute` method that proposes an action to the Guardrail.
- [ ] Define `src/agents/StrategicNode.ts` (Central Brain): Handles goal decomposition and conflict resolution when the Guardrail rejects an `AutonomousArm`'s action.
- [ ] Create specialized mock agents extending `AutonomousArm` (e.g., `FinanceAgent.ts`, `LogisticsAgent.ts`).

## Orchestration Simulation (Testing the Architecture)
- [ ] Create `src/main.ts`. Instatiate the `Blackboard`, `AuditLogger`, `GuardrailPerimeter`, `StrategicNode`, and specialized agents.
- [ ] Publish a high-level goal to the Blackboard (e.g., `PROCESS_COMPLEX_VENDOR_INVOICE`).
- [ ] Simulate the Strategic Node breaking it down into subtasks (`VALIDATE_INVOICE_LOGISTICS`, `APPROVE_INVOICE_FINANCE`).
- [ ] Simulate the `LogisticsAgent` executing successfully.
- [ ] Simulate the `FinanceAgent` proposing a budget violation, the `GuardrailPerimeter` intercepting it, and the `StrategicNode` gracefully handling the failure.
