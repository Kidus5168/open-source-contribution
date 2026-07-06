Open Source Contribution — AutoGPT
Project
AutoGPT — a platform for building, deploying, and running continuous AI agents, including an agent marketplace and library.

Forked to: https://github.com/Kidus5168/AutoGPT

Selected Issue
#9879 — Marketplace data should be downloaded with Agent
https://github.com/Significant-Gravitas/AutoGPT/issues/9879

Problem Summary
When a user downloads an agent from the marketplace into their personal library, the agent shows up with stale information rather than the details it was published with. Two specific gaps:

Wrong title. The downloaded agent displays the creator's original title from their own library instead of the title the agent was published under on the marketplace.
Missing images. The agent's marketplace image is not downloaded, so the library entry is missing the artwork users saw on the marketplace.
Expected behavior: A downloaded agent should appear in the user's library exactly as it was presented on the marketplace — with the current marketplace title and image intact.

Why This Matters
The download flow should carry over the marketplace-facing presentation data (title and image), not the creator's internal library record. This is a data-mapping issue in the agent download path.

Status
Phase I — issue selected, project forked, interest comment posted.
