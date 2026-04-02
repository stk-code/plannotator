# Improve plannotator event API structure and maintainability

## Goals
- Fix the `diffType` propagation bug in the shared code review event flow.
- Align the shared event API with actual supported behavior.
- Reduce brittleness around session context handling.
- Ensure new modules are covered by TypeScript checking.
- Remove duplicated browser/bootstrap helpers where practical.

## Plan

1. **Centralize shared browser action helpers**
   - Extract asset loading, browser open/wait/stop lifecycle, and shared action helpers into a single internal module.
   - Make both command handlers and event listeners call the same action entrypoints.

2. **Fix code review option propagation**
   - Pass the requested `diffType` through consistently to `runGitDiff(...)` and `startReviewServer(...)`.
   - Preserve `cwd` and `defaultBranch` behavior.

3. **Tighten the shared event API contract**
   - Either plumb request payload fields like `origin` through to the server calls or remove them from the public request types.
   - Keep only fields that are actually supported by the implementation.
   - Update README documentation to match the final contract.

4. **Improve session-context handling**
   - Avoid a single fragile mutable global context if there is a cleaner way within the extension lifecycle.
   - At minimum, make the current-session assumption explicit and isolate it in one place.

5. **Restore typecheck coverage**
   - Update `apps/pi-extension/tsconfig.json` so `plannotator-events.ts` and any new shared helper module are included.

6. **Sanity check the refactor**
   - Re-scan the modified files for dead code / unused exports.
   - Verify command handlers still produce the same user-facing behavior.
