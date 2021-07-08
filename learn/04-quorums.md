# Quorums

## Overview

Recall that FBA protocols implement a non-unanimous consensus mechanism by grouping nodes into groups known quorums. In general, a quorum is the minimum number of people who must participate in a vote in order for a certain proposal to be executed. In the early stage of the Bosagora platform, a quorum for resolution was set to one third of the total members. However, this can be adjusted later to reflect the average participation rate.

Initially, quorums were based on Stellar’s consensus protocol (SCP). But it has a weak point. The quorums on Stellar are configured manually by each node maintainer. This can lead to too much centralization if the quorums aren’t configured properly. Bosagora addresses that shortcoming by using *quorum balancing* (see below).

The rules for generating quorums on the Bosagora platform are as follows:

+ Nodes with a bigger stake have a higher chance of being included in other nodes’ quorums.
+ The probability of inclusion is equal to the percentage of a node’s stake compared to the stake of other nodes in the network. For example: If validators A, B, C stake 100K, 200K, 300K each, then validator C has a greater chance of being included in a node’s quorum configuration compared to A and B.
+ Randomness ensures that each node does not have the exact same quorum layout, as that would lead to too much centralization.
+ At specified intervals, the quorums are shuffled.

There are several checks to make sure quorums are generated correctly:

+ There is a check that threshold values are correct.
+ There is a quorum intersection check adapted from Stellar. It verifies there is sufficient quorum intersection in a given quorum configuration.
+ There is a verification that network splits are not possible with the given quorum configuration.

## Quorum Slicing




## Quorum Balancing
