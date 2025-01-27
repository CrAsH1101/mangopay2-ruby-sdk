## [3.6.0] - 2021.08.10
## Added

- You can now update and view a Recurring PayIn Registration object. To know more about this feature, please consult the documentation [here](https://docs.mangopay.com/guide/recurring-payments-introduction).
- To improve recurring payments, we have added new parameters for CIT : DebitedFunds & Fees. To know more about this feature, please consult the documentation [here](https://docs.mangopay.com/endpoints/v2.01/payins#e1053_create-a-recurring-payin-cit)

## [3.5.0] - 2021.06.10
## Added

We have added a new feature **[recurring payments](https://docs.mangopay.com/guide/recurring-payments)** dedicated to clients needing to charge a card repeatedly, such as subscriptions or payments installments.

You can start testing in sandbox, to help you define your workflow. This release provides the first elements of the full feature.

- [Create a Recurring PayIn Registration object](https://docs.mangopay.com/endpoints/v2.01/payins#e1051_create-a-recurring-payin-registration), containing all the information to define the recurring payment
- [Initiate your recurring payment flow](https://docs.mangopay.com/endpoints/v2.01/payins#e1053_create-a-recurring-payin-cit) with an authenticated transaction (CIT) using the Card Recurring PayIn endpoint
- [Continue your recurring payment flow](https://docs.mangopay.com/endpoints/v2.01/payins#e1054_create-a-recurring-payin-mit) with an non-authenticated transaction (MIT) using the Card Recurring PayIn endpoint

This feature is not yet available in production and you need to contact the Support team to request access.

## Accepted PRs

- Add support for refund creation
- Allow to fetch UBO Declaration without User ID



## [3.4.0] - 2021.05.27
## Added

### Instant payment

Mangopay introduces the instant payment mode. It allows payouts (transfer from wallet to user bank account) to be processed within 25 seconds, rather than the 48 hours for a standard payout.

You can now use this new type of payout with the Ruby SDK.

Example :

```ruby
bankwire = MangoPay::PayOut::BankWire.get_bankwire(payout['Id'])
# where payout['Id'] is the id of an existing payout
```

Please note that this feature must be authorized and activated by MANGOPAY. More information [here](https://docs.mangopay.com/guide/instant-payment-payout).

### Accepted PRs

- Add support to create refunds for PayIn, Transfer and PayOut transactions.
- ResponseError object improvement

## [3.3.0]
## Fixed

### IBAN for testing purposes

⚠️ **IBAN provided for testing purpose should never be used outside of a testing environement!**

More information about how to test payments, click [here](https://docs.mangopay.com/guide/testing-payments).

### Others

- Adding missing json require in log requests filter spec. Thank you @Vin0uz
- Extend fetch_wallet and create_payout API. Thank you @peterb

## Added

Some of you use a lot the [PreAuthorization](https://docs.mangopay.com/endpoints/v2.01/preauthorizations#e183_the-preauthorization-object) feature of our API. To make your life easier, we have added three new events :

- PREAUTHORIZATION_CREATED
- PREAUTHORIZATION_SUCCEEDED
- PREAUTHORIZATION_FAILED

The goal is to help you monitor a PreAuthorization with a [webhook](https://docs.mangopay.com/endpoints/v2.01/hooks#e246_the-hook-object).

*Example: If a PreAuthorization is desynchronized, when the status is updated, you will be able to know it.*



## [3.2.0]
## Added

### On demand feature for 3DSv2

> **This on-demand feature is for testing purposes only and will not be available in production**

#### Request

We've added a new parameter `Requested3DSVersion` (not mandatory) that allows you to choose between versions of 3DS protocols (managed by the parameter `SecureMode`). Two values are available:
* `V1`
* `V2_1`

If nothing is sent, the flow will be 3DS V1.

The `Requested3DSVersion` may be included on all calls to the following endpoints:
* `/preauthorizations/card/direct`
* `/payins/card/direct`

#### Response

In the API response, the `Requested3DSVersion` will show the value you requested:
* `V1`
* `V2_1`
* `null` – indicates that nothing was requested

The parameter `Applied3DSVersion` shows you the version of the 3DS protocol used. Two values are possible:
* `V1`
* `V2_1`




## [3.1.0]
- 3DS2 integration with Shipping and Billing objects, including FirstName and LastName fields
  The objects Billing and Shipping may be included on all calls to the following endpoints:
  - /preauthorizations/card/direct
  - /payins/card/direct
  - /payins/card/web
- Enable Instant Payment for payouts by adding a new parameter PayoutModeRequested on the following endpoint /payouts/bankwire
  - The new parameter PayoutModeRequested can take two differents values : "INSTANT_PAYMENT" or "STANDARD" (STANDARD = the way we procede normaly a payout request)
  - This new parameter is not mandatory and if empty or not present, the payout will be "STANDARD" by default
  - Instant Payment is in beta all over Europe - SEPA region
- Add test on new payout mode PayoutModeRequested
## [3.0.37] - 2020-10-30
- Card Validation endpoint fully activated
- added pre authorizations transactions method
- added new methods for client bank accounts and payouts
- Send headers for different api calls

## [3.0.36] - 2020-08-28
- Forces TLS version to 1.2

## [3.0.35] - 2020-08-24
- Improvement to Net::ReadTimeout handling
- "User-agent" format in the headers changed, aligned to other assets 👤

## [3.0.34] - 2020-06-25
### Added
- This SDK is now GooglePay-ready ! Feel free to ask our lovely support for more infos about its activation.
- `UBODeclaration` is now directly available through its ID.
- If a bankwire is done from a UK bankaccount on one of your `BankingAlias`, you could find its `AccountNumber` on `GET /payins/` response
- You can now send a `Culture` parameter for Paypal PayIns. Thanks to it, payment page can be displayed in various languages.

### Changed
- `PAYLINEV2`value for Payin Web has been added on `TemplateURLOptions` object. You now should use it instead of `PAYLINE` for page customization.

### Fixed
- Missing filters parameters have been added
- You can now send headers in update requests
- Loggers have been enhanced

## [3.0.33] - 2019-09-23
### Added
- ApplePay `Payin` functions are now available. More info about activation to come in the following weeks...
### Changed
- GET EMoney method now supports year and month parameters. More info on our [docs](https://docs.mangopay.com/endpoints/v2.01/user-emoney#e895_view-a-users-emoney)

## [v3.0.32] - 2019-06-19
### Added
- New UBO Declaration system (more info [here](https://docs.mangopay.com/endpoints/v2.01/ubo-declarations#e1024_the-ubo-declaration-object))
### Changed
- Paypal buyer account email that has been used is now available for Payin Paypal
- Your `HeadquartersPhoneNumber` can now be updated for your client account directly from our API
### BREAKING
- `APIKey` is now replacing `passphrase` property for credentials. You must update it by updating to 3.0.32 SDK version and upper ones.


 