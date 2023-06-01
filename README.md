# Streamlit Chronometer Error
This repo is meant to reproduce an error while running an asynchronous chronometer in Streamlit.

## Summary
Running this code locally works fine while the deployed version on Streamlit Cloud does not.

## Reproducible Code Example
It can be found at `chronometer.py`.

## Steps To Reproduce
Go to the deployed version:

[![Streamlit App](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://chronometer-error.streamlit.app/)

Then follow these steps:
- Click on "Start" button
- Click on "Stop" button
- Click on "Save" or "Delete" buttons
- Click on "Start" button again
- Click on "Stop" button again

The following error message will pop up:
```
Traceback (most recent call last):

  File "/home/appuser/venv/lib/python3.10/site-packages/streamlit/runtime/scriptrunner/script_runner.py", line 565, in _run_script

    exec(code, module.__dict__)

  File "/app/streamlit-chronometer/chronometer.py", line 78, in <module>

    asyncio.run(run_chronometer(container))

  File "/usr/local/lib/python3.10/asyncio/runners.py", line 44, in run

    return loop.run_until_complete(main)

  File "/usr/local/lib/python3.10/asyncio/base_events.py", line 649, in run_until_complete

    return future.result()

  File "/app/streamlit-chronometer/chronometer.py", line 14, in run_chronometer

    while st.session_state["chrono_running"] and run_time < MAX_TIME - TIMESTEP_REAL:

  File "/home/appuser/venv/lib/python3.10/site-packages/streamlit/runtime/state/session_state_proxy.py", line 90, in __getitem__

    return get_session_state()[key]

  File "/home/appuser/venv/lib/python3.10/site-packages/streamlit/runtime/state/safe_session_state.py", line 104, in __getitem__

    raise KeyError(key)
KeyError: 'chrono_running'
```

And the stop time will always fail to be captured from now on.

## Debug info
- Streamlit version: 1.19
- Python version: 3.10
- Operating System: Windows

## Additional Comments
If you try to use `streamlit==1.20+`, you would obtain the same error locally as well. I found out this is due to the `fastReruns` default value which has become `true` in these releases. If you overwrite the value to `false` within `.streamlit/config.toml` file, you would solve the problem locally but it will persist on the deployed version. Using `streamlit==1.19` overcomes this problem.