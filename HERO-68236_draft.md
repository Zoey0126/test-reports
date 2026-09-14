# Simple Smart Station - Fail to leave standalone mode if unable to connect to master server upon launcher starts Test Report

## Functional Testing

### 1.Leave Standalone Mode After Connection Recovery

#### 1.1.Leave Standalone Mode Passes After Master Server Connection Is Recovered

**Prerequisite(s):**
1. A Simple Smart Station is under standalone mode
2. The connection between the smart station and the master server can be blocked and restored for testing

**Step(s):**
1. While the smart station is unable to connect to the master server, restart the launcher service
2. Log in to the smart station after the launcher restarts
3. Restore the connection to the master server and confirm connectivity
4. Click Leave standalone mode on the smart station

**Reproduction Result(s):**
1. After restarting the launcher while the master server was unreachable, clicking Leave standalone mode showed the dialog box Fail to connect to server master even after the connection was recovered, and the station could not leave standalone mode

**Fix Result(s):**
1. Leave standalone mode succeeds after the connection to the master server is recovered
2. No Fail to connect to server master dialog is shown when the master server is reachable
3. The smart station leaves standalone mode and returns to normal (non-standalone) operation

### 2.Standalone Mode Entry and Exit

#### 2.1.Launcher Starts Into Standalone Mode When Master Server Is Unreachable

**Prerequisite(s):**
1. The connection between the smart station and the master server is blocked
2. The smart station launcher service can be restarted

**Step(s):**
1. Restart the launcher service while the master server is unreachable
2. Log in to the smart station
3. Check the operating mode of the station

**Reproduction Result(s):**
1. The launcher could start and the station could work in standalone mode, but the connection failure state was captured in a way that later prevented leaving standalone mode after recovery

**Fix Result(s):**
1. The launcher starts successfully and the smart station enters or stays in standalone mode when the master server is unreachable
2. The station records its connection state correctly so that standalone mode can be left once the master server is reachable

#### 2.2.Leave Standalone Mode on a Normal Connection

**Prerequisite(s):**
1. The smart station is under standalone mode
2. The connection to the master server is available the whole time

**Step(s):**
1. Log in to the smart station
2. Click Leave standalone mode while the master server is reachable

**Reproduction Result(s):**
1. Leaving standalone mode on a continuously available connection worked as expected before the fix and is used as the baseline behaviour

**Fix Result(s):**
1. Leave standalone mode succeeds while the master server connection is available
2. The station switches to normal mode without any error dialog

### 3.Leave Standalone Mode While Master Server Still Unreachable

**Prerequisite(s):**
1. The smart station is under standalone mode
2. The connection to the master server is blocked

**Step(s):**
1. Log in to the smart station while the master server is unreachable
2. Click Leave standalone mode
3. Restore the connection and click Leave standalone mode again without restarting the launcher

**Reproduction Result(s):**
1. Before the fix, a failed attempt could leave the station stuck so that even a later attempt after recovery still showed Fail to connect to server master

**Fix Result(s):**
1. While the master server is unreachable, a clear failure message is shown and the station remains in standalone mode
2. After the connection is recovered, clicking Leave standalone mode again succeeds without restarting the launcher

### 4.Regression Scope

**Prerequisite(s):**
1. The fixed build is deployed on the Simple Smart Station and the master server environment

**Step(s):**
1. Retest the full standalone mode lifecycle: entering standalone mode, operating in standalone mode and leaving standalone mode with the master server reachable
2. Retest the launcher service restart with the master server reachable and unreachable
3. Retest smart station login after launcher restart in both connection states
4. Retest the connection recovery flow: block the connection, restart the launcher, restore the connection and leave standalone mode

**Fix Result(s):**
1. All standalone mode entry, operation and exit scenarios pass without the Fail to connect to server master error when the master server is reachable
2. Launcher restart and login flows behave as before, confirming no regression is introduced by the fix
