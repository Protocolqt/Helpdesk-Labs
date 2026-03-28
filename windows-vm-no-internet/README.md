
# VirtualBox Windows VM – No Internet (Root Cause: Incorrect Adapter Mode)

## Objective
Restore internet connectivity for a Windows VM.

---

## Symptoms

![CMD Failure](screenshots/Symptom-CMD.png)

- No internet access in browser
- `ping 8.8.8.8` failed

---

## Investigation

![Adapter Setting](screenshots/VM-Adapter-Settings.png)
### Network Analysis

- `ipconfig /all` showed no usable default gateway
- System initially had an APIPA address (169.254.x.x), indicating DHCP was not reached
- Failure to ping `8.8.8.8` confirmed this was not just a DNS issue
- This pointed to a Layer 3 connectivity problem rather than application-level failure
- Checked IP configuration
- Identified incorrect VirtualBox adapter setting (Host-Only)

---

## Root Cause
The VM was configured with a Host-Only adapter, which allows communication only between the host and VM. It does not provide routing to external networks or internet access.

This prevented the system from obtaining proper network configuration for internet connectivity.

---
### Troubleshooting Path

1. Tested external connectivity with `ping 8.8.8.8`
   - Result: Failed
   - Meaning: Not a DNS issue, likely network connectivity

2. Checked IP configuration using `ipconfig /all`
   - Observed missing/invalid gateway or APIPA address
   - Meaning: DHCP or routing issue

3. Considered possible causes:
   - DNS misconfiguration ❌ (ruled out by ping test)
   - DHCP failure ⚠️ (possible symptom, not root cause)
   - Virtual network misconfiguration ✅ (likely)

4. Investigated VirtualBox network settings
   - Found adapter set to Host-Only

Conclusion:
The issue originated from incorrect virtualization network configuration, not guest OS misconfiguration.

---

## Resolution

![Fix Applied](screenshots/Resolution.png)

- Switched adapter to NAT
- Renewed IP configuration

---

## Verification
- VM obtained valid IP configuration after switching to NAT
- Default gateway was present
- `ping 8.8.8.8` succeeded
- Browser successfully accessed external websites

---

## Skills Demonstrated
- Network troubleshooting
- VirtualBox configuration
- Root cause analysis

## Key Takeaways
- APIPA addresses indicate DHCP failure at the time of assignment
- Successful ping to an external IP helps differentiate DNS vs connectivity issues
- VirtualBox adapter mode directly impacts network routing behavior
- Always verify gateway presence before assuming DNS is the issue
