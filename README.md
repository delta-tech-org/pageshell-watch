# pageshell-watch

The outside watcher of the PageShell platform: a GitHub Actions schedule, on GitHub's runners, that requests each
URL of `targets.txt` every 5 minutes. It is the alarm that still rings when the host, the proxy or the database is
down and nothing on the platform can speak.

- **Down** (a target fails three attempts, 20 s each): it opens the issue `Platform down` (label `outage`) listing
  the failing targets, and posts to Discord when the repository secret `DISCORD_DEADMAN_WEBHOOK` is set.
- **Still down**: nothing more; the open issue is the state.
- **Recovered**: it comments on the issue, closes it and posts the recovery once.

People who watch this repository get GitHub's notification for the issue. A scheduled run is best effort: GitHub
may start it a few minutes late. The `keepalive` job commits `heartbeat` once a month, so the schedule of a quiet
public repository stays enabled.
