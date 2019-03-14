# Changelog

## [unreleased]

#### Added
- Included status in `POST /admin/users/` and `PUT /admin/users/{identifier}/` endpoints docs with the options `pending`, `active` and `inactive`.
- Filtering support added to the `GET /admin/users/` endpoint
- Filtering support added to the `GET /admin/transactions/` endpoint
- `type` to user and transaction responses
- `reference-number` to the user model and user responses
- Create Bank EFT deposits at `POST /admin/deposits/bank-eft/`
- List Bank EFT deposits at `GET /admin/deposits/bank-eft/`
- Get Bank EFT deposits at `POST /admin/deposits/bank-eft/{identifier}/`
- REST actions for the old Card Deposits are now also available at `/admin/deposits/card/`

#### Changed
- Changed `first_name` and `last_name` to NOT be required for `POST /auth/register/` and `POST /admin/users/` endpoints.
- Changed the old deposit model to `card_deposit`. REST actions at `/admin/deposits/` serve exclusively card deposits. These are also available at `/admin/deposits/card/` as added above

#### Fixed
- Registering a user via the `/auth/register` endpoint no longer returns a 400 if the `groups` field is included in the POST payload.
- Fixed a bug causing withdrawals to timeout.

## [2.7.0] - 2019-03-05

#### Added
- Token endpoint for users to list, create and delete their tokens.
- Pagination added to the `GET /admin/users/` endpoint.
- Transaction aggregation endpoint.

#### Fixed
- Fixed a bug preventing the card confirm 3ds page from displaying.

## [2.5.0] - 2019-01-28

#### Added
- Admin API endpoints for companies to create and update their fees.
- Admin API endpoint to delete a user.
- User API endpoint to set a bank account to primary.
- Documentation for new API endpoints.
- Settings to allow international phone number registration.
- New automated unit tests for improved code coverage.
- Transaction endpoint for admins and users to retrieve transactions, of any type.
- Pagination support for the new transaction endpoints.
- User registration endpoints.
- User login endpoints.
- `type` and `status` fields now included in user responses.
- User Transfer endpoints for create, list and get transfers.
- User Withdrawal endpoints for create, list and get withdrawals.

#### Changed
- Phone number validation now uses the libphonenumber library.
- All phone numbers must be in international format, i.e. prepended with +COUNTRY_CODE (e.g. +27 for ZA).

#### Removed
- Removed API endpoint for updating bank accounts.

#### Fixed
- Fixed a bug where transactions could not be paged on the dashboard
- Fixed a bug preventing cards from being registered due to the formatting of the expiry month field.  
