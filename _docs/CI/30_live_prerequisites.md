---
title: Prerequisites
subtitle:
tags: []
author: hakim
layout: doc_ci
---

#### Sections in this article
{:.no_toc}
* TOC
{:toc}

# Prerequisites

<!--
- [Prerequisites](#prerequisites)
  - [Technical Assessment](#technical-assessment)
  - [Choice of Specific features](#choice-of-specific-features)
  - [Indexer](#indexer)
    - [Choosing the right Indexer solution for your your project](#choosing-the-right-indexer-solution-for-your-your-project)

-->

***

Cryptocurrencies projects have always been technically complex, involving state-of-the-art technologies and designs in cryptography and decentralization, among others.
Nowadays, they are also functionally complex: it’s no longer just about sending and receiving a single currency, we see more and more individual projects with their own purpose and identity, their own innovative features, such as staking, rewards, tokens, smart contracts, on-chain governance, etc…

Therefore, every new coin integration calls for a preliminary analysis, to gather all information about the coin before being able to start coding, making sure nothing is left in the shadow. In order to do so, this pre-analysis has to cover both the functional and technical aspects of the coin.  

***
## Technical Assessment

<!-- ------------- Image ------------- -->
<img width="395" height="548" src="../../../uploads/images/technical_assessment.png" style="float:right">
<!-- --------------------------------- -->

<!-- ------------- Image ------------- -->
[//]: 488 × 677
[//]: !technical_assessment(/../../uploads/images/technical_assessment.png)
<!-- --------------------------------- -->

The Technical assessment phase consists firstly in acquiring a thorough understanding of the blockchain protocol, considering the gereric and specific aspects.

To do this, please download and fill in the form provided, to guide you along.
[technical_assessment_v1.pdf](../../files/technical_assessment_v1.pdf)   


Secondly, choices must be made concerning the Specific features.

And lastly, special care and careful consideration must be given to providing the Indexer Solution.

The output of this phase is a detailed proposition of choices made and discussed with Ledger for agreement, before coding starts. This is done at a joint meeting to agree on the Acceptance Plan.

***
## Choice of Specific features

The technical choices made concerning Specific features regard all staking operations, including rewards operations.


## Indexer

[//]: To know more about the indexing service requirements, please have a look at  **LINK**
[//]: 786 × 525

<!-- ------------- Image ------------- -->
<img align="centre" width="471" height="315" src="../../../uploads/images/blockchain_infra.png" >
<!-- --------------------------------- -->

Both UTXO and account-based blockchains can contain a serious amount of transactions for any given account. Because Ledger Live shows the complete list of operations to the end user, the synchronisation process can take a long time, especially if addressing the blockchain node directly.

To improve performances and allow fast, incremental and optimistic synchronisation, Ledger Live relies on indexing services.

Ledger runs its own indexing services but can also rely on third parties to operate and maintain these services. In the latter case, and to ensure the end user privacy, Ledger adds a reverse proxy layer between the end-user and the indexing service.

### Choosing the right Indexer solution for your your project

<!-- ------------- Image ------------- -->
![indexer_decision_tree.png](../../uploads/images/indexer_decision_tree.png)
<!-- --------------------------------- -->

***
