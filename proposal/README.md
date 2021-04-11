# Calendar View proposal

## Goal
- Provide better visibility over the full state of the DAG since the start date.
- Make it easier to track the progress of large backfills at a glance.

# Screenshots & Usage
![screenshot](./view.png)

- The states of all the dag runs are aggregated and displayed per day.
- The "state" displayed for given day depends on the state of the dag runs for that day. the dag run with the
highest-priority state will determine the state displayed for that day.
I.e. if a dag run has has failed for that day, but another dag run has succeeded, the day sill be displayed as "failed".
The order of priority is the default one:
```
failed > upstream_failed > up_for_retry > up_for_reschedule > queued > scheduled > sensing > running > shutdown > removed > no_status > success > skipped
```
- For each day, a tooltip shows the number of dag runs for each state for that day:

![screenshot](./tooltip.png)
- Clicking on a day redirects to the tree view for that day to show all the dag runs until the end of that day.

# Implementation
- New `/calendar` endpoint in `views.py`.
- New `calendar.html` template and `calendar.js` script.
- The calendar JS logic uses d3 & moment (already airflow dependencies, not version bump required).
