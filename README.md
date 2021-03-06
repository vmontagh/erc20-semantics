ERC20-K: Formal Executable Specification of ERC20
=================================================

Author: [Grigore Rosu](http://fsl.cs.illinois.edu/grosu)

Date: 6 December 2017

## Abstract

The [ERC20 standard](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20-token-standard.md)
is one of the most important standards for the implementation of tokens
within Ethereum smart contracts.
ERC20 provides basic functionality to transfer tokens and to be approved so
they can be spent by another on-chain third party.
Unfortunately, ERC20 leaves several corner cases unspecified, which makes it
less than ideal to use in the formal verification of token implementations.
ERC20-K is a complete formal specification of the ERC20 standard.
Specifically, it is a *formal executable semantics* of a *refinement of the
ERC20 standard*, using the [K framework](http://kframework.org).

ERC20-K clarifies what data (accounts, allowances, etc.) are handled by
the various ERC20 functions and the precise meaning of those functions on such
data.
ERC20-K also clarifies the meaning of *all* the corner cases that the ERC20
standard omits to discuss, such as transfers from yourself to yourself
or transfers that result in arithmetic overflows, following the most natural
implementations that aim at minimizing gas consumption.

Being executable, ERC20-K can also be tested for increased confidence.
Driven by the semantic rules that form ERC20-K, as well as by their side
conditions, we manually but systematically produced and provide a test-suite
consisting of several dozens of tests which we believe cover all the corner
cases.
We encourage you to analyze these tests and use them to test your
implementations.
Please contribute with more tests if you think that we left any interesting
behaviors uncovered.


## Motivation

[KEVM](https://github.com/kframework/evm-semantics) makes it possible to
rigorously verify smart contracts at the Ethereum Virtual Machine (EVM) level
against higher level specifications.
The most requested property to verify so far was *correctness of ERC20 tokens*
written in languages like Solidity or Viper.
But what does "correctness of ERC20 tokens" really mean?
When formal verification is sought, a property (or specification) that the
code must satisfy must be available.
Moreover, such a specification should be unambiguous and capable of
answering all the questions regarding corner-case behaviors.
To our knowledge, there was no such formal specification for ERC20, or
for ERC20 variants, available at the time of this writing (December 2017).

The existing ERC20 specifications are either too informal to serve as a formal
specification for verification purposes, or they are not executable, incomplete
and use unnecessarily advanced logical formalisms.
For example, the Ethereum
[wiki](https://theethereum.wiki/w/index.php/ERC20_Token_Standard) and
[github](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20-token-standard.md)
variants are rather informal and imprecise, in spite of being called
*formalized* by a
[coindesk article](https://www.coindesk.com/ethereums-erc-20-token-standard-formalized),
and so are the ERC20 descriptions by
[McDonald](https://medium.com/@jgm.orinoco/understanding-erc-20-token-contracts-a809a7310aa5),
[Seibel](https://medium.com/@james_3093/ethereum-erc20-tokens-explained-9f7f304055df),
[McKie](https://medium.com/blockchannel/the-anatomy-of-erc20-c9e5c5ff1d02),
etc., as well as the
[Stackoverflow](https://ethereum.stackexchange.com/questions/16714/what-are-the-key-criteria-for-meeting-the-erc20-token-standard/16725)
explanations.
On the other hand, when actual formalizations as a mathematical theory were
attempted, such as
[McRainface](https://dapphub.github.io/LLsai/token)'s, they are paper-based
(i.e., not executable) and using logical formalisms that are hard to understand
by most developers, such as
[linear logic](https://plato.stanford.edu/entries/logic-linear/).

## Structure

The main ERC20-K specification is defined and extensively commented in this
file:

* [erc20.md](erc20.md)

The following folder contains unit tests for the ERC20-K specification,
that is, small programs that exercise the various ERC20 functions in various
contexts.
For that, a programming language needs to be first defined on top of ERC20-K:

* [tests](tests)

## Contribute

We welcome contributions!
The easiest way to contribute is to add more tests to existing languages in the
folder [tests](tests).
A more involved way to contribute is to add new languages under [tests](tests),
together with their own unit tests.
It would be nice to cover a variety of language paradigms, such as more
imperative language, object-oriented, functional, and even logical programming
languages.
Finally, you can adopt ERC20-K as *the standard specification of ERC20* when
testing and verifying smart contracts and this way:
(1) we as a community converge on one
formal standard for token correctness, as opposed to each group having
different versions and opinions about what correctness means, most likely
missing some corner cases and thus allowing vulnerabilities; and
(2) we as a community improve the test suite, for the benefit of us all.

## Acknowledgments

We thank the K team who defined the 
[KEVM](https://github.com/kframework/evm-semantics) semantics
(see
[technical report](https://www.ideals.illinois.edu/handle/2142/97207), too)
and verified smart contracts for ERC20 compliance.
It is their effort that inevitably led to the quest for a formal specification
of ERC20.
In particular, we would like to thank
[Philip Daian](http://pdaian.com/) for brainstorming and help with
understanding the corner cases of ERC20.

We also warmly thank [IOHK](http://iohk.io) not only for their generous funding
support of both [KEVM](https://github.com/kframework/evm-semantics) and
[IELE](https://github.com/runtimeverification/iele-semantics), but also for
the stimulating technical discussions we had with their research team.
Discussions about IELE and about the planned separation between the settlement
and the computational layers in Cardano, in particular, led to the question of
whether the ERC20 specification can be defined in a more abstract way that
makes it usable in combination with computational layers different from EVM.
