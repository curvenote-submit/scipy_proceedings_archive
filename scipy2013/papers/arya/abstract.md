---
title: 'DMTCP: Bringing Checkpoint-Restart to Python'
description: DMTCP (Distributed MultiThreaded CheckPointing) is a mature
  checkpoint-restart package. It operates in user-space without kernel
  privilege, and adapts to application-specific requirements through plugins.
abstract: 'DMTCP (Distributed MultiThreaded CheckPointing) is a mature
  checkpoint-restart package. It operates in user-space without kernel
  privilege, and adapts to application-specific requirements through plugins.
  While DMTCP has been able to checkpoint Python and IPython
  "from the outside" for many years, a Python
  module has recently been created to support DMTCP. IPython support is included
  through a new DMTCP plugin. A checkpoint can be requested interactively within
  a Python session, or under the control of a specific Python program. Further,
  the Python program can execute specific Python code prior to checkpoint, upon
  resuming (within the original process), and upon restarting (from a checkpoint
  image). Applications of DMTCP are demonstrated for: (i) Python-based graphics
  using VNC; (ii) a Fast/Slow technique to use multiple hosts or cores to check
  one Cython computation in parallel; and (iii) a reversible debugger, FReD,
  with a novel reverse-expression watchpoint feature for locating the cause of a
  bug.'
---
