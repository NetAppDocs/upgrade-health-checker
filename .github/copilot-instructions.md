## Copilot instructions for Upgrade Health Checker documentation

### Repository overview
Product: Upgrade Health Checker

*Upgrade Health Checker* is an on-site advisory tool that analyzes *ONTAP* cluster data and generates upgrade-readiness reports before planned upgrades. The tool also supports fleet-level visibility through a generated dashboard and can be integrated into existing automation workflows.

### Repository structure
- `_include/` – Shared include-content directory reserved for reusable AsciiDoc fragments used across pages.
- `media/` – Static media assets used by documentation pages, including dashboard visuals.
- `.github/` – Repository automation metadata and Copilot instruction context for this documentation set.

### Product-specific context
**Architecture and components:**
- The `uhc` binary runs on a Linux VM and connects to ONTAP cluster management interfaces over HTTPS to perform pre-upgrade health checks.
- The tool uses cluster and *AutoSupport* data to evaluate upgrade readiness and produce findings such as blockers, risks, warnings, and skipped checks.
- Each execution creates a unique run directory under a runs path; outputs include logs plus report artifacts in HTML, JSON, and Excel formats.
- A static fleet dashboard is generated from run data at `runs/dashboard/index.html`; `--dashboard` regenerates it from existing runs without contacting clusters.
- Optional integrations include SMTP email delivery for reports/logs and scheduled automation (for example cron) for repeated runs across multiple clusters.

**Key concepts:**
- A *run* is one complete execution of *Upgrade Health Checker* for one or more clusters, with isolated output and timestamps.
- *Blockers* are critical findings that must be resolved before upgrade, while *warnings* and *at-risk* findings indicate lower-severity concerns that still affect planning.
- A cluster can appear as *Ready*, *Blocked*, *Error*, or *Stale* in the dashboard based on the latest available run status.
- Input resolution follows strict priority: CLI arguments first, then `config.yaml`, then interactive prompts.

**Naming conventions and terminology:**
- Command format uses the `uhc` executable with long-form flags such as `--cluster-ip`, `--target-ontap-version`, `--runs-path`, and `--dashboard`.
- Configuration keys use uppercase YAML sections and fields such as `APP.RUNS_PATH`, `CLUSTER.IP`, `CLUSTER.TARGET_ONTAP_VERSION`, `EMAIL.*`, `AUTO_UPDATE.ENABLED`, and `TELEMETRY.ENABLED`.
- Report filenames follow `uhc_<cluster-name>_<YYYYMMDDHHMMSS>` and are stored under a per-run `results/` directory.
- Documentation distinguishes *Upgrade Health Checker* (on-site, automation-friendly) from *Upgrade Advisor* (cloud-based, UI-centered).

### Typical user workflows
**Initial setup and first run:** Download binary from Automation Hub → Place on Linux VM with cluster/network access → Configure ONTAP service account and permissions → Optionally prepare `config.yaml` → Run connectivity checks and execute `uhc`

**Generate and review upgrade plan:** Provide cluster credentials and target release inputs → Accept EULA and run health checks → Collect run outputs in `runs/` → Open HTML/JSON/XLSX reports → Address blockers and warnings before upgrade

**Fleet monitoring and recurring operations:** Schedule repeated `uhc` runs for multiple clusters → Retain run history per cluster in unique run folders → Regenerate dashboard with `--dashboard` from stored runs → Filter/search fleet status to track readiness and stale results
