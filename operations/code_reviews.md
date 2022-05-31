# The "Yieldcurity" Standard

Opinionated **security**, **code quality** standards, and **code review policies and procedures** for **Solidity** smart contracts.  Inspired by and fully incorporating [The Solcurity Standard](https://github.com/Rari-Capital/solcurity).


### Code author creates pull request:
- Include a **meaningful description** explaining the change - highlight areas of **risk** or **complexity**
- Reference Github issue
- Include [hardhat-gas-reporter](https://hardhat.org/plugins/hardhat-gas-reporter.html) report.  If updating pre-existing code, include both before and after reports
- Include report from [SMTChecker](https://docs.soliditylang.org/en/v0.8.7/smtchecker.html) or other analysis if conducted
- Assign a **priority tag** to the PR if it needs to be done faster than 3 days
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
- Explain tests if they help understand the new code changes or if there is particular complexity in the tests themselves, otherwise, leave it to the reviewers to think about a testing approach on their own before reviewing the tests

### Reviewers:
- Reviewers should conduct an independent, thorough review
- Normally, the review should be completed within 3 days of being notified (faster if priority tagged)
- Refer to the [Solcurity smart contract code quality standards](https://github.com/Rari-Capital/solcurity)
- Some general  tips:
    - Construct a mental model of what you expect the contracts to look like before checking out the code.  Compare the architecture to your mental model. Look into areas that are surprising
    - Look at areas that can do **value exchange**
    - Look at areas that **interface with external** contracts and ensure all assumptions about them are valid
    - Create a threat model and make a list of theoretical high level attack vectors
    - Do another review from the perspective of every actor in the threat model
- Go through the Solcurity standards checklist for each of the following:
    - [Events](https://github.com/Rari-Capital/solcurity#events)
    - [Structs](https://github.com/Rari-Capital/solcurity#structs)
    - [Modifiers](https://github.com/Rari-Capital/solcurity#modifiers)
    - [Variables](https://github.com/Rari-Capital/solcurity#variables)
    - [Functions](https://github.com/Rari-Capital/solcurity#functions)
    - [External Calls](https://github.com/Rari-Capital/solcurity#external-calls)
    - [Static Calls](https://github.com/Rari-Capital/solcurity#static-calls)
- Consider these additional Yield security precautions:
    - Never ever allow users to call arbitrary contracts from our contracts. Either use auth or some kind of whitelist.
- After reviewing the code changes, see the [Code](https://github.com/Rari-Capital/solcurity#events), [Contract](https://github.com/Rari-Capital/solcurity#events), and [DeFi](https://github.com/Rari-Capital/solcurity#events) checklists
- Before reviewing tests, construct a mental model of how you would approach testing:
    - Are **realistic** scenarios/values used in tests?
    - Are all important **edge cases** and states being tested?
    - For bug fixes, is there a new test that would have caught the bug?
    - If there are any issues that should hold up merge, mark the PR as **Changes Requested**
    - **Approve** If completely comfortable with the changes

### Code Author obtains final approval and merges PR:
- Address any comments or changes requested, _be sure to mark comments as resolved_
- Notify reviewers if any changes are made to obtain final approval
- **Must have at least 1 approval prior to merge**
- Merge PR and close related Github issue

[Edit this page](https://github.com/yieldprotocol/docs-v2/edit/main/operations/code_reviews.md)