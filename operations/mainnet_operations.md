# Mainnet Operations

Standards to propose and execute **mainnet operations** on Yield v2 contracts.

### Proposal lead developer creates pull request:
- Include a **meaningful description** explaining the change - highlight areas of **risk** or **complexity**
- Reference Github issue or governance proposal
- Assign a **priority tag** to the PR if it needs to be done faster than 3 days
- Link the code to be run
- Specify input data to be used, preferably hardcoded in the linked script
- Specify how the operation can be verified after execution
- Specify if the operation is to be executed at a specific time after approval, or as soon as approved
- Fork mainnet and run the code through proposal, approval (impersonating multisig) and execution
- Assign reviewers
- Schedule a time for video review - add details (time/Zoom link) in PR description
- Consider inviting persons outside the smart contract team (ie front end) or non devs (marketing etc) when demoing a new feature or important change
- Notify invitees/reviewers

### Code author leads video review session:
- Record the session
- **Explain context** of change
- **Demo** the change if possible
- For bug fixes, explain or demo the broken version vs the fixed version
- Highlight areas of **risk or complexity**

### Reviewers:
- Reviewers should conduct an independent, thorough review
- Normally, the review should be completed within 3 days of being notified (faster if priority tagged)

### Code Author obtains final approval and merges PR:
- Address any comments or changes requested, _be sure to mark comments as resolved_
- Notify reviewers if any changes are made to obtain final approval
- **Must have at least 1 approval prior to merge**
- Register proposal in the Timelock
- Wait for governance approval
- Execute proposal in the Timelock
- Merge PR and close related Github issue