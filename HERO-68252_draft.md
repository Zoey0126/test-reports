# IP2COM - Recover USB printing after printer power-off; cleaner service stop; optional auto-restart for long disconnect Test Report

## Functional Testing

### 1.USB Printer Reconnect After Power Cycle

#### 1.1.Printing Resumes Automatically After Printer Power-Off and Power-On

**Prerequisite(s):**
1. A USB kitchen printer is connected through IP2COM and printing normally
2. The IP2COM service is running with default configuration

**Step(s):**
1. Power off the USB printer while it is idle
2. Wait a few seconds and power the printer back on
3. Send a print job from the POS

**Test Result(s):**
1. IP2COM reconnects to the printer automatically after power-on
2. The print job prints successfully without a manual IP2COM service restart

#### 1.2.Repeated Power Cycles and Short USB Disconnects Reconnect Reliably

**Prerequisite(s):**
1. A USB printer is connected through IP2COM and printing normally

**Step(s):**
1. Power the printer off and on several times in a row, sending a test print after each power-on
2. Unplug the USB cable briefly, plug it back in and send a test print
3. Repeat the plug/unplug cycle several times

**Test Result(s):**
1. The printer reconnects automatically after each power cycle or replug
2. Printing resumes every time without a manual IP2COM service restart
3. Reconnect after the device comes back is fast, using shorter reconnect waits first

### 2.Progressive Reconnect Retry Timing Applies to USB Only

**Prerequisite(s):**
1. A USB printer and a non-USB printer (for example a network printer) are both connected through IP2COM

**Step(s):**
1. Take the USB printer offline for a longer period and observe the reconnect retry pattern in the logs
2. Take the non-USB printer offline and observe its reconnect behaviour
3. Bring both devices back and verify printing

**Test Result(s):**
1. The USB printer uses shorter reconnect waits at first and longer waits if it stays offline
2. The non-USB printer keeps its previous reconnect handling and timing unchanged
3. Both printers resume printing normally after they come back

### 3.Disconnect Watchdog Optional Auto-Restart

#### 3.1.Watchdog Disabled by Default Keeps Retrying Without Service Restart

**Prerequisite(s):**
1. config.ini under [setup] has no restart_service_on_long_disconnect entry or the value is 0 (default)
2. A USB printer is connected through IP2COM

**Step(s):**
1. Power off the USB printer and keep it offline for a long period
2. Monitor the IP2COM service and the reconnect logs
3. Power the printer back on and send a test print

**Test Result(s):**
1. The Disconnect Watchdog stays off and the IP2COM service is not restarted automatically
2. Reconnect retry continues indefinitely while the device stays offline
3. Printing resumes when the printer is powered back on

#### 3.2.Watchdog Enabled Restarts the Service with Progressive Intervals

**Prerequisite(s):**
1. config.ini under [setup] has restart_service_on_long_disconnect=1
2. A USB printer is connected through IP2COM and can be kept offline for an extended period

**Step(s):**
1. Power off the USB printer and keep it offline
2. Monitor the IP2COM service restarts and the intervals between them, including the overnight behaviour
3. Power the printer back on after a restart and send a test print

**Test Result(s):**
1. The IP2COM service is restarted automatically only after reconnect keeps failing for a long time
2. The first restarts wait about 5 minutes each (for the first six), then intervals grow to 30 minutes, 1 hour, 2 hours, 4 hours and up to 8 hours, so the PC is not disrupted every few minutes
3. Printing resumes after the device returns and the service is running

#### 3.3.Existing restart_service_on_init_error Option Remains Unchanged

**Prerequisite(s):**
1. restart_service_on_init_error is configured per its existing behaviour
2. restart_service_on_long_disconnect is set to 0

**Step(s):**
1. Trigger an IP2COM initialization error covered by restart_service_on_init_error
2. Observe the service restart behaviour for the init error
3. Keep a USB device disconnected for a long time and observe that no long-disconnect restart happens

**Test Result(s):**
1. The init error restart behaviour is exactly as before the change
2. The long-disconnect watchdog does not trigger while it is off, confirming the two options remain separate

### 4.Clean Service Stop and Resource Release

**Prerequisite(s):**
1. IP2COM is running with at least one USB printer connected and print jobs have been processed

**Step(s):**
1. Stop the IP2COM service normally from the service control
2. Restart the service and send a test print
3. Force-stop the service while it is busy, restart it and print again

**Test Result(s):**
1. Stop and force-stop close printers, network connections and related work properly before exit
2. After each restart the service starts cleanly and printing works without leftovers from the previous session

### 5.Logging for Expected Disconnects

**Prerequisite(s):**
1. IP2COM is running with debug logging off
2. A USB printer is connected

**Step(s):**
1. Power the USB printer off and on a few times
2. Review the IP2COM logs for the disconnect events
3. Enable debug logging, repeat the cycle and compare the noise level

**Test Result(s):**
1. Expected disconnects are logged clearly with understandable reasons
2. With debug off, the logs contain less noise for normal disconnect and reconnect cycles

### 6.HTML Printer Spacing Fix

**Prerequisite(s):**
1. An HTML printer is configured with print content that contains spacing entities (for example the non-breaking space entity)

**Step(s):**
1. Print a document with spacing entities to the HTML printer
2. Compare the printed output spacing with the expected layout

**Test Result(s):**
1. Spacing is rendered correctly and the previous spacing defect is no longer visible in the HTML print output
