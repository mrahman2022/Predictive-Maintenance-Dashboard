# Hydraulic Fault Explorer

A standalone dashboard for exploring pump leakage in a hydraulic test rig. Select a recorded test cycle to see the pump’s recorded condition and compare its 60-second flow trace with a healthy reference.

Live Demo: https://predictive-maintenance-dashboard-hydraulicfaultexplorer.streamlit.app/

The public demo requires no username or password. Try cycles **1** and **220** to compare two recorded pump conditions.


## Architecture

```text
FS1.txt       ── flow readings ─┐
                               ├── Streamlit dashboard
profile.txt   ── test labels ────┘
```

The app matches the two files by row, with one row per test cycle. It builds the healthy reference by taking the median flow at each time point across stable cycles with all four components in their nominal condition.

## Dashboard

Hydraulic Fault Explorer is a one-screen dashboard for reviewing pump leakage in a hydraulic test rig. Select a test cycle to see its recorded leakage condition and compare its 60-second flow trace with a healthy reference.

Use **Test cycle** to change the displayed cycle. The headline summarizes the recorded pump condition, and the chart compares the selected cycle’s flow in L/min with the healthy reference.

| Test cycle | Recorded condition | Dashboard headline |
| --- | --- | --- |
| 1 | No leakage | No pump leak recorded |
| 220 | Severe internal leakage | Inspect the pump first |

- **Condition headline** summarizes the pump leakage label recorded for the selected cycle.
- **Selected cycle** shows the measured flow in litres per minute (L/min).
- **Healthy reference** shows the median flow at each time point across stable cycles where all four components were in their nominal condition.

## Work orders

The Explorer supports manual inspection triage. In a real maintenance workflow, a technician could use the displayed condition and flow trace to document why an inspection was requested.

If logging a work order, include the cycle number, recorded condition, and a screenshot of the flow comparison. Work-order creation, assignment, and tracking take place separately in the group's main dashboard or maintenance system.

## Expected results

| Recorded pump condition | Dashboard headline | Suggested follow-up | Demo cycle |
| --- | --- | --- | --- |
| No leakage | No pump leak recorded | Continue routine monitoring | 1 |
| Weak internal leakage | Check the pump | Review the flow trace and flag for follow-up if warranted | Select a cycle labeled weak leakage |
| Severe internal leakage | Inspect the pump first | Prioritize a pump inspection | 220 |

The recorded condition comes from the UCI dataset. The chart helps users see where the selected flow trace differs from the healthy reference.


## Dataset and scope

The UCI dataset contains 2,205 one-minute cycles from a hydraulic test rig. `FS1.txt` contains flow readings sampled at 10 Hz. The third column of `profile.txt` records pump leakage as none, weak, or severe.
Dataset: Helwig, N., Pignanelli, E., & Schütze, A. (2015).
[Condition monitoring of hydraulic systems](https://doi.org/10.24432/C5CW21),
UCI Machine Learning Repository. The included `FS1.txt` and `profile.txt`
files are distributed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

The app displays those recorded labels. Use the chart to explore differences in flow between cycles.

## Limitations

- **Recorded test data.** The demo uses historical test-rig cycles. It has no live equipment feed.
- **Known conditions.** The headline reads the recorded leakage label. The app has no model for diagnosing leakage from flow or forecasting failures.
- **One sensor view.** The chart displays `FS1` flow. Other component conditions can also affect readings, so a difference from the reference does not establish the cause on its own.
- **Single reference.** The median trace summarizes healthy cycles. The chart does not show the range of normal variation.
