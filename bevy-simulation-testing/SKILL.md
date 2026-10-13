---
name: bevy-simulation-testing
description: "Test Bevy ECS or Avian physics through deterministic App updates without rendering or windows."
---
# Simulation testing

Use for state and physical outcomes. Screenshot or visual correctness requires a renderer-backed test instead.

## Version gate

Resolve the project's Bevy version/features first. For Bevy 0.20 work or an upgrade to it, read [0.20 task checks](references/bevy-0.20.md) before implementation; retain version-matched APIs for older projects.

## Procedure

1. Define the observable behavior and smallest real feature plugin boundary. Build an App with only the required scheduling, time, input, and asset resources; retain the systems under test.
2. Control simulated time explicitly and seed randomness. Complete the target release's plugin readiness/finish/cleanup lifecycle before driving updates. Done when fixed ticks advance predictably and startup has run.
3. Use direct ECS mutation for fixture setup; drive the behavior under test through its normal systems/input. Assert after the relevant consumers or fixed ticks, not after an arbitrary sleep.
4. Cover success, rejection, teardown, and repeated execution. Give evolving simulations a bounded settling/deadline criterion and tolerances tied to physical scale.
5. Run focused tests and repository checks. Completion requires deterministic state outcomes and a failure-capable regression, with rendered/OS coverage explicitly excluded.

## Physics branch

For Avian gravity, collision, settling, or containment, read [Avian tests](references/avian.md) before composing the test App.

## Resource boundary

Minimal plugin groups do not supply every subsystem's prerequisites. Add the asset/mesh/visibility resources required by the actual tested systems rather than substituting fake outcomes.
