# wxtop-health

Watches [wxtop](https://wxtop.space.fast/)'s shared cache, wxtop-cache (`https://wxtop-cache.space.fast`), from GitHub Actions.
It's a public repository so the minutes are free; the code it checks lives in the private wxtop repository (`cache/`).

- **health.yml**, every 5 minutes (GitHub runs schedules late or skips some when it's busy): asks the cache what wxtop's page asks it,
  with the page's Origin.
  1. `/api/stats` answers.
  2. A METAR (KJFK) answers: aviationweather.gov is only reachable through the cache.
  3. A deliberate miss (air quality at a point that changes every run) is fetched and answered.

  A failing check is tried again after 20 s; a second failure fails the run, and GitHub emails whoever last changed the schedule.
  A readable error from the cache itself on the miss (the service was slow or refused) is a warning, not a failure.
  Each run's summary page lists every check and the cache version that answered.
- **keepalive.yml**, monthly: GitHub switches off scheduled workflows in a public repository after 60 days without activity;
  this switches the health check back on.

Run either by hand: Actions → the workflow → Run workflow.
