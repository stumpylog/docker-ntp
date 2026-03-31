# CHANGELOG

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Changed

- Dependabot now groups all updates for each ecosystem into a single PR using `patterns: ["*"]`
- Add 7-day cooldown to Dependabot updates for both GitHub Actions and Docker ecosystems

### Security

- Pin all GitHub Actions to full commit SHAs to prevent supply chain attacks
- Add `persist-credentials: false` to all `actions/checkout` steps to prevent credential leakage into artifacts
- Replace `ncipollo/release-action` with native `gh release create` to reduce third-party action dependencies
- Fix template injection risk in release job by moving `github.ref_name` and step outputs into environment variables before use in `run:` steps
- Correct `aquasecurity/trivy-action` tag from non-existent `0.34.1` to `v0.34.0`

## [1.5.0] - 2026-01-26

### Added

- Image now builds for `linux/arm64` platform as well

### Changed

- Updates base Alpine image to 3.23.2

## [1.4.0] - 2025-12-09

### Added

- Dockerfile linting via Hadolint action
- Docker image security scanning via Trivy
- Validation of the CHANGELOG format

### Changed

- Updates base Alpine image to 3.23.0

### Fixed

- dependabot group name for the Docker updates

## [1.3.0] - 2025-07-26

### Changed

- Updates base Alpine image to 3.22.1
- Syncs upstream changes

### Fixed

- dependabot targeted the wrong branch

## [1.2.1] - 2025-05-31

### Fixed

- Fixed the changelog release date

## [1.2.0] - 2025-05-31

### Changed

- Updates base Alpine image to 3.22.0

## [1.1.0] - 2025-01-21

### Changed

- Updates base Alpine image to 3.21.2

### Added

- Dependabot configuration for Docker

## [1.0.0] - 2024-11-27

### Added

- This changelog file
- GitHub Actions workflow to build and push the image to ghcr.io
- Dependabot configuration for actions
- `pre-commit` hooks
- EditorConfig configuration file
- Updated base Alpine image to 3.20.3
