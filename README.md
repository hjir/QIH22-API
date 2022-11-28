# Quantum Internet Hackathon 2022: APIs for Quantum Protocols Challenge

In this challenge, the focus is on Application Programming Interfaces (APIs) for quantum protocols. In order to make quantum protocols useful, they need to be able to communicate with classical applications. Within this challenge, you can undertake several tasks that will bring us closer to this goal.

Quantum network protocols are usually implemented using low-level primitives such as quantum gates, entangled qubits, measurements etc. This makes them very difficult to integrate with real applications such as web browsers. The current situation would be equivalent to requiring every single web application to write its own code to establish a TCP connection, craft the HTTP requests, follow any redirects if necessary, and then parse the response. This is unnecessary overhead for the vast majority of programs and thus most of them will use well established libraries like [libcurl](https://curl.se/libcurl/) or [requests](https://requests.readthedocs.io/en/latest/).

It would be much better if established quantum protocols instead provided a similar programmatic API facilitating its use in high-level applications. One such example already exists for Quantum Key Distribution: [ETSI QKD API](https://www.etsi.org/deliver/etsi_gs/QKD/001_099/004/02.01.01_60/gs_QKD004v020101p.pdf). This challenge is all about designing and implementing such APIs for quantum protocols.

To check out an example of what this challenge is about, see Bruno Rijsman's [report](https://brunorijsman.github.io/openssl-qkd/) on the QKD integration within OpenSSL for the RIPE Quantum Internet Hackathon in 2019.

## The Challenge

The challenge is split up into three parts:

1. Design and implement an API for a quantum protocol
2. Integrate an API within an already existing open-source app
3. Connect an API to QNE-ADK

Each of the parts can be done independently and they require different skills; you can choose which one is best aligned with your own interests or split up the sub-challenges within your team. Note that multiple teams can also collaborate together and focus on finishing all the sub-challenges for the same quantum protocol.

## 1. API Design & Implementation

An example of an API written for a quantum protocol is the Quantum Key Distribution (QKD) API written by [ETSI](https://www.etsi.org/); it can be found [online](https://www.etsi.org/deliver/etsi_gs/QKD/001_099/004/02.01.01_60/gs_QKD004v020101p.pdf) (v2.1.1). This can be a great inspiration for you when working on this sub-challenge, and you can also use the format of the ETSI API definition for your own API design.

The goal is to take any quantum protocol from the [Quantum Protocol Zoo](https://wiki.veriqloud.fr/index.php?title=Main_Page) and design an API for it. Think about which functionality it provides and how can the communication between the quantum and classical parts be generalised.

After having defined such an API, you can work on writing a mockup of it. You can do this in any programming language. If you can already think of an application in which such a quantum protocol could be used, you can pick the application's programming language to allow for easier integration in the future.

In this repository, we also provide an example of a mockup written for the ETSI QKD API v1.1.1 (see [`qkd_api/mock`](qkd_api/mock)). This mockup is written in C, so another possible task for you is to re-implement in your programming language of choice (e.g. Python).

**Note on the different versions of ETSI QKD APIs**: See the README in [`qkd_api`](qkd/api/README.md) for more details on this.

## 2. API Integration

The next step in using quantum protocols within classical applications is to integrate the API within an actual application. The goal here is to take a quantum protocol and integrate it within an open-source application or library (e.g. Telegram or OpenSSL). The output of this sub-challenge can be a demonstration of a running application.

You can follow this step using the API and its mockup that you implemented as part of the previous sub-challenge, or you can make use of the ETSI QKD API and the mockup provided in this repository. Note that if you decide to work on QKD integration, the main difference to most commonly used algorithms is that QKD is symmetric and the keys will be provided by some server, not by the user.

## 3. API and QNE-ADK Connection

Up until now, the API has only been mocked and it has not been communicating with any simulators or real quantum hardware. In the future, this will be one of the goals: providing an implementation of the API to actually connect classical applications and quantum protocols executed on quantum hardware.

For now, this sub-challenge gives you an opportunity to help with those efforts on a smaller scale. The idea is to try to implement an API of your choosing using calls to QNE-ADK. To give a more concrete example, for the QKD API this would be developing a QKD application using QNE-ADK in such a way that it provides the method calls defined in the API.

Of course, QNE-ADK currently only provides a command-line interface. This might be cumbersome to use for the actual API implementation. It is already helpful if you define what needs to change in QNE-ADK to allow for more straightforward implementation of the APIs for quantum protocols. The output of this sub-challenge can therefore be a written document in which you define these needs. You can also check out the [repository of QNE-ADK](https://github.com/QuTech-Delft/qne-adk) yourself and get started on it. Don't hesitate to reach out to the QNE team during the hackathon to talk about this more!
