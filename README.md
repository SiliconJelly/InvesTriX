# News

> **Note to readers:** This is one of my major start-up concept, which was founded August 2021.
> The project repos are in the process of being migrated. All projects will remain available for use here until the migration to a new GitHub Organization is complete.

<a href="https://developers.investrix.com">
	<img width="200" src="https://investrix.carrd.co/assets/images/image04.png" alt="InvesTriX Logo" />
</a>

---

[![InvesTriX Rust Crate Documentation (main)](https://img.shields.io/badge/docs-main-59f)](https://investrix.github.io/investrix/)
[![License](https://img.shields.io/badge/license-Apache-green.svg)](LICENSE)
[![grcov](https://img.shields.io/badge/Coverage-grcov-green)](https://ci-artifacts.investrix.com/coverage/unit-coverage/latest/index.html)
[![test history](https://img.shields.io/badge/Test-History-green)](https://ci-artifacts.investrix.com/testhistory/investrix/investrix/auto/ci-test.yml/index.html)

InvesTriX Core implements a decentralized, programmable database which provides a financial infrastructure that can empower billions of people.

## Note to Developers
* InvesTriX mobile app is a prototype.
* The APIs are constantly evolving and designed to demonstrate types of functionality. Expect substantial changes before the release.
* We’ve launched a testnet that is a live demonstration of an early prototype of the InvesTriX Blockchain software.

## Contributing

To begin contributing, [sign the CLA](https://investrix.com/en-US/cla-sign/). You can learn more about contributing to the InvesTriX project by reading our [Contribution Guide](https://developers.investrix.com/docs/community/contributing) and by viewing our [Code of Conduct](https://developers.investrix.com/docs/policies/code-of-conduct).

## Getting Started

### Learn About InvesTriX
* [Welcome](https://developers.investrix.com/docs/welcome-to-investrix)
* [InvesTriX Protocol: Key Concepts](https://developers.investrix.com/docs/core/investrix-protocol)
* [Life of a Transaction](https://developers.investrix.com/docs/core/life-of-a-transaction)
* [JSON-RPC SPEC](json-rpc/json-rpc-spec.md)

### Try InvesTriX Core
* [My First Transaction](https://developers.investrix.com/docs/core/my-first-transaction)
* [Getting Started With Move](https://developers.investrix.com/docs/move/move-introduction)

### Technical Papers
* [The InvesTriX Blockchain](https://developers.investrix.com/docs/technical-papers/the-investrix-blockchain-paper)
* [Move: A Language With Programmable Resources](https://developers.investrix.com/docs/technical-papers/move-paper)
* [State Machine Replication in the InvesTriX Blockchain](https://developers.investrix.com/docs/technical-papers/state-machine-replication-paper)

### Blog
* [InvesTriX: The Path Forward](https://developers.investrix.com/blog/2019/06/18/the-path-forward/)

## Community

* Join us on the [InvesTriX Discourse](https://community.investrix.com).
* Ask a question on [Stack Overflow](https://stackoverflow.com/questions/tagged/investrix).
* Get the latest updates to our project by signing up for our [newsletter](https://developers.investrix.com/newsletter_form).

## License

InvesTriX Core is licensed as [MIT License](https://github.com/NJR4beatZ/investrix/blob/main/LICENSE).


# NOTICE : **Consider this project a philanthropic envision.**

I will continued to keep this documentation up to date and it's almost complete locally, I was happy to answer questions that wouldn't potentially expose a private, internal API, unreleased feature, or sensitive 3rd party data. 

Feel free to contact me about InvesTrix.

To Everyone : Bad actors, profiteers, and scammers have created a situation that puts us all at risk of having our accounts closed. I suggest you find ways to distance yourselves. These people have pushed InvesTriX into making drastic changes to their API, shutting down established partnerships, and enforcing their customer agreements. If you haven't been contacted by an automated support agent yet for using the API directly, don't be too surprised if you are in the near future.

I'm not sure what will happen to this repo yet but I have had a good relationship with RH HQ since there was a waiting list to even create an account and I intend to stay in their good graces. I will not plug one here but commission-free firms exist that are designed to be accessed via their API. If that's in line with your plans, use one of those firms.

-------


# **N.B. : I am open for pull requests or other contributions of endpoints feel free to drop your commits!**

# Unofficial Documentation of InvesTriX Trade's Private API

Table of Contents:

- [Table of Contents](#table-of-contents)
- [Introduction](#introduction)
	- [Development](#development)
- [API Security](#api-security)
- [API Error Reporting](#api-error-reporting)
- [Pagination](#pagination)
	- [Semi-Pagination](#semi-pagination)

## See also

- [Authentication.md](Authentication.md) for methods related to user authentication, password recovery, etc.
- [Banking.md](Banking.md) for bank accounts & ACH transfers methods
- [Order.md](Order.md) for order-related functions (placing, cancelling, listing previous orders, etc.)
- [Options.md](Options.md) for options related endpoints
- [Quote.md](Quote.md) for stock quotes
- [Fundamentals.md](Fundamentals.md) for basic, fundamental data
- [Instrument.md](Instrument.md) for internal reference to financial instruments
- [Watchlist.md](Watchlist.md) for watchlists management
- [Account.md](Account.md) talks about gathering and modifying user and account information
- [Settings.md](Settings.md) covers notifications and other settings
- [Markets.md](Markets.md) describes the API for getting basic info about specific exchanges themselves
- [Referrals.md](Referrals.md) is all about account referrals
- [Statistics.md](Statistics.md) exposes the new social/statistical endpoints
- [Tags.md](Tags.md) exposes the new categorizing endpoints

Things I have yet to organize are in [Unsorted.md](Unsorted.md)

# Introduction

[InvesTriX](https://investrix.com/) is a commission-free, online platform for youth to get involved in the real world of finance. As you would expect, being an online service means everything is handled through a request that is made to a specific URL. We run our user's and investor's real money from a decentrlized settings via blockchain technologies. And our transparency has provided us with more than 70% of pre-launch commitment with our future users. Therefore, the potential is on the bright sides.

# API Security

The HTTPS protocol is used to access the InvesTriX API. Transactions require security because most calls transmit actual account informaion. SSL Pinning is planned to be used in the official Android and iOS apps to prevent MITM attacks; you would be wise to do the same at the very least. Make sure to mail us, if you wanna be one of thse shortlisted beta tasters for our vision 2025 mobile application.

Calls to API endpoints make use of two different levels of authentication:

1. **None**: No authentication. Anyone can query the method.
2. **Token**: Requires an authorization token generated with a call to [log in](Authentication.md#logging-in).

Calls which require no authentication are generally informational ([quote gathering](Quote.md#quote-methods), [securities lookup](#instrument-methods), etc.).

Authorized calls require an `Authorization` HTTP Header with the authentication type set as `Token` (Example: `Authorization: Token 40charauthozationtokenherexxxxxxxxxxxxxx`).

# API Error Reporting

The API reports incorrect data or improper use with HTTP status codes and JSON objects returned as body content. Some that I've run into include:

| HTTP Status | Key                | Value | What I Did Wrong |
|-------------|--------------------|-------|------------------|
| 400         | `non_field_errors` | `["Unable to log in with provided credentials."]` | Attempted to [log in](#logging-in) with incorrect username/password |
| 400         | `password`         | `["This field may not be blank."]`                | Attempted to [log in](#logging-in) without a password |
| 401         | `detail`           | `["Invalid token."]`                              | Attempted to use cached token after [logging out](#logging-out) |
| 400         | `password`           | `["This password is too short. It must contain at least 10 characters.", "This password is too common."]`                                                       | Attempted to [change my password](#password-reset) to `password` |

...you get the idea. Letting you know exactly what went wrong makes the API almost self-documenting so thanks InvesTriX.

# Pagination

Some data is returned from the InvesTriX API as paginated data with `next` and `previous` cursors already in URL form.

If your call returns paginated data, it will look like this call to `https://api.investrix.com/instruments/`:

```
{
    "previous": null,
    "results": [{
        "splits" : "https://api.investrix.com/instruments/42e07e3a-ca7a-4abc-8c23-de49cb657c62/splits/",
        "margin_initial_ratio" : "1.0000",
        "url" : "https://api.investrix.com/instruments/42e07e3a-ca7a-4abc-8c23-de49cb657c62/",
        "quote" : "https://api.investrix.com/quotes/SBPH/",
        "symbol" : "SBPH",
        "bloomberg_unique" : "EQ0000000028928752",
        "list_date" : null,
        "fundamentals" : "https://api.investrix.com/fundamentals/SBPH/",
        "state" : "active",
        "tradeable" : true,
        "maintenance_ratio" : "1.0000",
        "id" : "42e07e3a-ca7a-4abc-8c23-de49cb657c62",
        "market" : "https://api.investrix.com/markets/XNAS/",
        "name" : "Spring Bank Pharmaceuticals, Inc. Common Stock"
    },
        ...
    ],
    "next": "https://api.investrix.com/instruments/?cursor=cD04NjUz"
}
```

To get the next page of results, just use the `next` URL.

## Semi-Pagination

Some data is returned as a list of `results` as if they were paginate but the API doesn't supply us with `previous` or `next` keys.
