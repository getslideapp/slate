# Changelog

## [unreleased]

#### Added
- Transaction aggregation endpoint.

#### Fixed
- Registering a user via the `/auth/register` endpoint no longer returns a 400 if the `groups` field is included in the POST payload.

## [2.7.0] - 2019-03-05

#### Added
- Token endpoint for users to list, create and delete their tokens.
- Pagination added to the `GET /admin/users/` endpoint.

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
