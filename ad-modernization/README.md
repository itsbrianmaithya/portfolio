# Active Directory Modernization — Vendor Engagement & Hybrid Identity Hardening

## Context
A mid-size enterprise engaged an external vendor to modernize its on-premises Active Directory environment and hybrid identity setup (Microsoft Entra ID / Azure AD). I acted as the internal engineering counterpart for the engagement — responsible for environment provisioning, access governance, and validating the security of the vendor's requested access before any work began.

## Role
Internal AD / Infrastructure Engineer — technical point of contact for the vendor, owner of access provisioning and identity security decisions for the engagement.

## What I Did

**1. Vendor access risk review**
The vendor's kickoff request asked for standing Enterprise Admin (on-prem AD) and Global Administrator (Microsoft 365) access to be granted *before* a scoping meeting had defined what work actually required it. I recognized this sequencing — full privileged access requested immediately post-contract, ahead of an agreed workplan — as a pattern worth independently verifying, confirmed the engagement's legitimacy through an out-of-band channel, and pushed the access request back into the correct order: scope first, access sized to scope second.

**2. Access governance redesign**
Rather than granting standing high-privilege accounts, I implemented:
- Just-in-time Enterprise Admin elevation — added to the privileged group only for scheduled maintenance windows, removed after, with directory audit alerting on group membership changes
- Microsoft Entra ID PIM for Global Admin as an *eligible* (not active) role, requiring justification and approval with time-boxed activation
- Conditional Access and MFA enforcement on all privileged accounts
- Break-glass accounts kept fully outside these policies and under internal control only

**3. Environment provisioning**
Built three on-premises servers from scratch for the vendor's use. Rather than exposing all three to external access, I configured the firewall so external RDP was permitted to a single entry server only; the vendor then RDP'd internally from that server to the other two. This reduced the number of external entry points into the environment from three to one, while still giving the vendor full access to do their work.

**4. Secure data handoff**
Compiled and shared the requested domain information via an expiring, access-logged link rather than email attachment, in line with data handling and confidentiality requirements for the engagement.

## Tools & Technologies
Active Directory · Microsoft Entra ID · Privileged Identity Management (PIM) · Conditional Access · Windows Server · Firewall / RDP access control

## Outcome
- Vendor engagement proceeded on governed, auditable, time-boxed access instead of standing high-privilege grants
- Environment provisioned and hardened ahead of the vendor's kickoff timeline, with external attack surface minimized to a single controlled entry point
