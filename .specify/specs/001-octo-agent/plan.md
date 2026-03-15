# Technical Plan: 001-octo-agent Core Architecture

## Architecture Overview
A Node.js\/TypeScript backend implementing an asynchronous event-driven architecture (EDA) based on the Blackboard pattern, designed to coordinate distributed AI agents (Small Language Models and Large Language Models) without relying on third-party orchestration frameworks.

## Component Breakdown
1. **The Blackboard (`Blackboard.ts`):**
   - The central nervous system and single source of truth.
   - Implements an append-only event log (e.g., in-memory array or simple SQLite table for persistence/auditability).
   - Exposes an Event Emitter or Pub\/Sub mechanism (`subscribe`, `publish`).
   - Every event MUST contain: `id`, `timestamp`, `agentId`, `eventType`, `payload`, and `status`.

2. **The Strategic Core (`StrategicNode.ts`):**
   - The "Brain" (LLM-powered).
   - Subscribes to high-level goals or complex conflicts on the Blackboard.
   - Decomposes goals into sub-tasks and publishes them to the Blackboard.
   - Monitors overall progress and resolves deadlocks.

3. **Specialized Agents (`AutonomousArm.ts`):**
   - The "Arms" (SLM-powered).
   - Abstract class defining the interface for domain-specific agents (e.g., `FinanceAgent`, `LogisticsAgent`).
   - Subscribes to specific task types on the Blackboard.
   - Executes tasks asynchronously and proposes results back to the Blackboard.

4. **Guardrail Middleware (`GuardrailPerimeter.ts`):**
   - The "Digital Instincts".
   - Intercepts proposed actions before they are finalized on the Blackboard or executed externally.
   - Enforces hard constraints: `BudgetValidator`, `EndpointAllowlist`, `EthicsFilter`.
   - Rejects non-compliant actions, publishing an `ACTION_REJECTED` event for the Strategic Core to handle.

5. **Observability & Audit (`AuditLogger.ts`):**
   - Taps into the Blackboard event stream.
   - Formats and persists events for compliance (SOX, GDPR, HIPAA).
   - Provides methods to replay the event log for debugging root-cause analysis.

## Implementation Steps
1. **Phase 1: Foundation (The Nervous System)**
   - Setup TypeScript project.
   - Implement the `Blackboard` class (Event Bus + State Log).
   - Implement the `AuditLogger` to trace all Blackboard activity.

2. **Phase 2: Digital Instincts (Guardrails)**
   - Implement the `GuardrailPerimeter` middleware.
   - Create mock validators (Budget, Allowlist) to test interception logic.

3. **Phase 3: The Cephalopod (Agents)**
   - Define the `StrategicNode` and `AutonomousArm` base classes.
   - Create two specialized mock agents (e.g., `FinanceAgent`, `LogisticsAgent`).

4. **Phase 4: Orchestration (The Simulation)**
   - Create a `main.ts` entry point.
   - Simulate a complex workflow (e.g., "Process a complex vendor invoice").
   - Demonstrate the Strategic Core decomposing the task, the Arms executing it, the Guardrails intercepting a bad action, and the system gracefully recovering.
