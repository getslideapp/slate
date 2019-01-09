# Changelog

## [Unreleased]

### Added
- Added this changelog section.
- Admin API endpoints for companies to create and update their fees.
- Admin API endpoint to set delete a user.
- User API endpoint to set a bank account to primary.
- Documentation for new API endpoints.
- Setting to allow international phone number registration.
- New automated unit tests for improved code coverage.

### Changed
- Phone number validation now uses the libphonenumber library.
- All phone numbers must be in international format, i.e. prepended with +COUNTRY_CODE (e.g. +27 for ZA).
- Rehive library was updated from version 1.1.9 to 1.1.11

### Removed
- Removed API endpoint for updating bank accounts.
