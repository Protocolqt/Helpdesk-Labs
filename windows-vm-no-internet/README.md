
# Windows VM Lost Internet (VirtualBox)

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
