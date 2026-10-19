# Changelog

All notable changes. Dates are build dates; every result is **synthetic demand, uncalibrated**.
Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/); versions follow the milestones
of `PROJECT.md` section 12.

## [Unreleased]

### Added
- **Film voice-over (Track I, MOVIES_V2):** narration scripts `movies/specs/<film>.narration.yaml`
  spoken offline by Kokoro-82M (Apache-2.0) in a pinned venv (`make voice-setup`); shots stretch
  to fit speech; voice track at -16 LUFS muxed as AAC with a soft subtitle track and
  `movies/<film>.srt`; burnt-in HUD subtitles; showreel headlines; `--no-voice`, `--no-subtitles`;
  `python -m scripts.movies.narrate validate` rejects numbers that are not in the docs.
- **Map annotations in the films:** OSM street names (English first, Arabic second line) with
  road-class priority, collision avoidance and fades; district and landmark labels
  (`movies/landmarks.yaml`); compass; scale bar; legend keys; callouts at crashes, floods,
  motorcades, evacuation exits and contraflow; "fictional demo parameters" footer.
- **Obhur evacuation demo:** window `obhur` (Obhur Creek mouth near the Jeddah Tower site) and
  the fictional scenario `evac_obhur` with strategist results in docs/evacuation.md.

### Removed
- The waterfront evacuation demo in the tahlia window (`evac_corniche_hamra`) and its film
  scenario: the public evacuation demo is Obhur.

### Fixed
- `tzdata` is a direct dependency, so prayer times (`ZoneInfo("Asia/Riyadh")` in
  `sim/daytypes/prayer.py`) work without a system time zone database (bare containers, Windows).

## [0.1.0] - 2026-10-17: first public release

### Added
- **Open-source release readiness (Track G):** CI on ubuntu-latest (ruff, pytest on the demo
  window, web typecheck and build; actions pinned to commit SHAs; uv and npm caches), a manual
  `demo-smoke` workflow (`make demo` with the servers probed over HTTP), a GitHub Pages workflow
  for the static project page `site/` (`scripts/build_site.sh`), `SECURITY.md`,
  `CODE_OF_CONDUCT.md`, issue and PR templates, `CITATION.cff`, good first issues in
  `docs/issues/`, `docs/release-audit.md` (licences, data, secrets, history) and
  `docs/release-checklist.md`. README: what it is, hero GIF, 60-second quickstart, films table,
  headline results with caveats, hardware, project status, documentation index. NOTICE: basemap
  (OpenFreeMap / OpenMapTiles), produced works and third-party software.
- **Loop tunnels (Track D):** corridor siting, tunnel network variants, shuttle and private-EV
  policies, sweep and recommendation (C1 Al-Salamah - King Abdulaziz University), `docs/loop.md`.
- **Films (Track E):** eight films and a showreel rendered from the runs with headless deck.gl
  and ffmpeg (`make movies`), GIF previews and stills in `docs/img/movies/`, `docs/movies.md`.
- **One-command demo:** `make demo` (setup, Jeddah network, demo runs, API and viewer on
  http://localhost:5173), `./run_app.sh` (serve `runs/`, `--detach`, `TT_API_PORT` /
  `TT_WEB_PORT`), `make city` (network variant, zones and the 10 % weekday 24 h baseline),
  `make test`, `make stop`, `make screenshots`.
- `python -m sim.demo` and `sim/config/demo.yaml`: the demo runs (Tier B smoke, demo events
  scenario, strategist, evacuation, a demo-scale policy comparison, a one-hour mid-day Tier A city
  smoke), skipped when already done, recorded in `runs/demo-manifest.json`.
- `sim/scenarios/demo_events.yaml` (fictional accident under heavy rain, motorcade, ambulance).
- `sim.runner --smoke --begin` (smoke runs at any time of day).
- `docker compose up --build` runs the same steps in a `demo` container before the `api` and `web`
  services; `.dockerignore`; osmium-tool in the image.
- Docs: `docs/architecture.md` (Mermaid), `docs/demo-script.md` (5-minute walkthrough),
  `CONTRIBUTING.md`, this changelog, README quickstart, release screenshots `docs/img/final-*.png`.

### Fixed
- Docker image: the eclipse-sumo Linux binaries need `libatomic1`, `libx11-6`, `libxext6`,
  `libxrender1` and `libgl1` (compose had never been run before M9); `demo` and `api` share one image.
- `make setup` runs `npm ci` only when `web/package-lock.json` changed.

### Known issues
- City baseline teleports 50.5 % of vehicles (network capacity, missing OSM lane counts); the lane
  audit from imagery is blocked on accuracy. Results are synthetic demand, uncalibrated, 10 % sample.
- Population hash differs between macOS and Linux (geometry float last bits); viewer compare is
  limited to runs on the same network; open items in `docs/status/final.md` and `docs/issues/`.

## [0.0.8] - 2026-10-16: integration of tracks A, B and C
- Merged M2-M8 into `main`; `sim.runner --scenario evac_*` dispatches to the evacuation runner;
  `--baseline --routing-threads --gzip`.
- API contract v1.1: reader-side normaliser for scenario/evacuation sub-runs, strategist
  recommendations and SQLite decisions, policy district effects, gzipped edge data, sparse bin cache.
- Merged assumptions register (256 ids); `pillow` as a direct dependency.

## [0.0.7] - 2026-10-16: M8 calibration path (dormant)
- Count and travel-time adapters, commercial-API stubs, `routeSampler.py` wrapper, GEH report.
  No real counts exist; synthetic round trip only.

## [0.0.6] - 2026-10-16: M7 evacuation
- Zone evacuation event with phased release, contraflow network edit, signal plans, clearance
  metrics (T50/T90/T100) and evacuation strategies ranked by the strategist.

## [0.0.5] - 2026-10-16: M6 emergency strategist
- Snapshot fork (`saveState`/`loadState`), candidates (k-shortest routes, preemption, rescue lane,
  closures), parallel what-ifs, ranking with side-effect penalties, plain-language explanations,
  action classification, hash-chained SQLite decision log, approve CLI and viewer panel.

## [0.0.4] - 2026-10-16: M5 focus windows and events
- Four default windows, service-road graft, Tier A to Tier B link with `cutRoutes.py`, Webster
  retiming of guessed signals, vType mix; accident, rain, flood, motorcade and emergency-trip
  events through TraCI with window KPIs.

## [0.0.3] - 2026-10-16: M4 policy lab
- Policy schema and transforms (remote work, staggered hours, day types, bus line, cordon pricing,
  network change, ride-hail share), incremental re-routing runner, matched controls, policy report
  with district effects and corridors, sweep tool.

## [0.0.2] - 2026-10-16: M2 population and baseline, M3 viewer
- Zones, synthetic population, activity chains, day types with prayer times, modes, trips,
  `duaIterate` assignment with a route cache, 24 h meso baseline at 10 %, meso-vs-micro fidelity
  test.
- File-backed FastAPI and the React + deck.gl + MapLibre viewer: edge-bin playback with a time
  slider, difference mode, district choropleth, Tier B trips, overlays, strategist panel, jobs.
- Lane audit from imagery: built, **blocked** on calibration accuracy (60 % held-out precision).

## [0.0.1] - 2026-10-15: M0 skeleton and M1 Jeddah network
- Repo, `uv`, Docker, CI, licence and NOTICE, demo window.
- `sim.network.build_city` from the Geofabrik GCC extract with the Jeddah polygon, reviewed OSM
  fix list, districts and buildings GeoJSON, meso smoke run (0.144 % teleports).
