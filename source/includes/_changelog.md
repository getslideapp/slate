# Changelog

## [Unreleased]

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
- `type` field to the `User` model.
- `status` field to the `User` model.
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
