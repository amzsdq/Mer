# Bootstrap Contract v0

This file defines only invariants required to load and validate the durable control plane.

1. GitHub owns dynamic Goal/Plan/state.
2. The automation prompt is a deployed bootstrap copy, not a second dynamic-state authority.
3. control/active.json is the normal wake entrypoint.
4. Canonical prompt drift is detectable through prompt-manifest.json.
5. Missing or invalid control files fail closed for project-state mutation: preserve scheduler recoverability, report the bootstrap fault, and do not fabricate task state.
6. Historical evidence is not part of the normal bootstrap read path.
7. Details that can safely change between wakes belong outside this file.
