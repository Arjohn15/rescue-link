# Low-Bandwidth and Offline Strategy

## Overview

AlertoHub's resident-facing web application is optimized for low-bandwidth and offline conditions using Progressive Web App (PWA) techniques and service workers. This ensures residents can submit rescue requests even when their internet connection is weak or completely unavailable during a disaster. The optimization applies to the resident side only.

---

## Approach

### Progressive Web App (PWA)

The resident-facing Next.js app is configured as a PWA using `next-pwa` and Workbox. This enables:

- Offline access to the rescue request form
- Cached map tiles for location selection
- Background sync for queued requests

### Service Workers

A service worker intercepts network requests and applies caching strategies:

| Resource              | Strategy                                  | Reason                              |
| --------------------- | ----------------------------------------- | ----------------------------------- |
| Rescue request form   | Cache first                               | Always available offline            |
| Map tiles             | Cache first                               | Pre-cached on first load            |
| API requests (submit) | Network first with offline queue fallback | Ensures latest data when online     |
| Static assets         | Cache first                               | No need to re-fetch unchanged files |

### Offline Request Queue

When a resident submits a request while offline:

```
Resident submits request with no signal
        ↓
Request data is saved to local storage on the device
        ↓
System displays message: "Your request has been saved
and will be sent automatically when your connection is restored"
        ↓
Service worker monitors connectivity
        ↓
Signal detected
        ↓
Queued request is sent automatically
        ↓
Resident receives tracking code via confirmation message
```

---

## Tech Stack

| Tool                     | Purpose                                                |
| ------------------------ | ------------------------------------------------------ |
| `next-pwa`               | PWA configuration and service worker setup for Next.js |
| Workbox                  | Service worker caching strategies                      |
| IndexedDB / localStorage | Local queue storage for offline requests               |
| Background Sync API      | Automatic retry when connectivity is restored          |

---

## Scope

Low-bandwidth optimization applies to the resident side only. The admin dashboard and rescuer portal require stable connectivity to function correctly.

| Interface                      | Offline Support            |
| ------------------------------ | -------------------------- |
| Resident (rescue request form) | Yes                        |
| Resident (tracking page)       | Partial (cached data only) |
| Admin dashboard                | No                         |
| Rescuer portal                 | No                         |

---

## Real-World Context

The Philippine context presents specific connectivity challenges during disasters:

- Typhoons and flooding frequently disrupt cellular towers
- Lower Bicutan, Upper Bicutan, Central Bicutan, and New Lower Bicutan may have limited 4G coverage during disaster conditions
- Signal congestion during mass emergencies reduces effective bandwidth

AlertoHub is designed to function on 2G/Edge networks, not just 4G/5G, directly addressing these constraints.

---

## Notes

- This strategy applies to the resident-facing app only.
- Future improvement: extend offline support to the rescuer portal for field use on low-signal deployments.
