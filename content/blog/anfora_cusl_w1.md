+++
title = 'Anfora at CUSL'
date = '2019-02-08'
author = 'Yábir García'
tags = ['anfora', 'cusl']
+++

## What is CUSL?

CUSL is the acronym for 'Concurso Universitario de Software Libre' or
Universitary contest of free software. I'm taking part on this contest
with Anfora.

## Week 1 (Feb 01 - Feb 08)

In this first week I've managed to achieve a lot of things.

### Client 

- We have basic support for a dark mode [99407d8](https://github.com/anforaProject/client/commit/99407d85b5aec65faec9d9d128ee69eb8672dd45) and [21465a7](https://github.com/anforaProject/client/commit/21465a72736c86e54b39a9de94c4e459d45df76d).
- Now some little issues with requests and undefined variables are fixed [e88d9c6](https://github.com/anforaProject/client/commit/e88d9c60f18585eb8523dc362f99eb4d1cb85925) and [21d5db6](https://github.com/anforaProject/client/commit/21d5db643ae653a94dcab8352a845c57200adb93)

### Server

- The best of the commits is this on were **AP federation** starts to work [3d4b890](https://github.com/anforaProject/anfora/commit/3d4b89040cb299d229b7b0e92950c1001f818649)

- Some issues with blocking endpoints were fixed [9a51b2b](https://github.com/anforaProject/anfora/commit/9a51b2b1f1df8d7016aae97759adabfc7bec40b0)
- A downgrade or update has been necessary for some packages after an issue updating them [5835a7a](https://github.com/anforaProject/anfora/commit/5835a74aa477ac0b946885cfcd0bae9b4c1055b9), [aa854b3](https://github.com/anforaProject/anfora/commit/aa854b362f380f42caf333996cee1d098e0d3795) and [4343214](https://github.com/anforaProject/anfora/commit/43432140f51381fa866e3c148428e47cc887a9a7)

### Others

This week I've experimented with image filters on python and I've
created this [public package](https://github.com/anforaProject/pyfilters.git) to apply some of the filters on
Instragram. There's a minor effects that is not finished but I'm
working on it.

## Week 2 (Feb 09- Feb 15)

This can be title the **black week**. I wanted to deploy a test
instance but It was a big failure. Mostly because I rushed the code
but I wanted to let the people test on the go.

## Server 

- The issues with nodeInfo were fixed
  [8e84a08](https://github.com/anforaProject/anfora/commit/8e84a084fa9ef9ae3bcca725d34b789e3fb79cf5). NodeInfo
  is an standard to discover platforms in the federated wolrd. More info [here](https://github.com/jhass/nodeinfo).
  
- We have new code to share the new statuses with followers
  [3cc3539](https://github.com/anforaProject/anfora/commit/3cc3539a5431f36bd95f6c0086d9c95baba94028).
  
- New API endpoint to share with the client when the server has registrations open [912b0f7b](https://github.com/anforaProject/anfora/commit/912b0f7b2aca15eadd2131f9fb9041fd9a05de4e).

- Now the creation of new users is handled async fixing a bug in the main thread were many requests were blocked [1d31fd2](https://github.com/anforaProject/anfora/commit/1d31fd2491d5ad8aaae8c93500118b106526bc42)

This week have tought me a lot of things and I'm taking a rest to go back with federation again!

## Week 3 (Feb 16 - Feb 22)

This week was to rest before going back to code. 
