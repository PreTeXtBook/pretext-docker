# Change Log

All notable changes to the PreTeXt-Docker image will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

Instructions: Add a subsection under `[Unreleased]` for additions, fixes, changes, and removals of features accompanying each PR.

## [Unreleased]

### Changed

- Use NVM to install latest version of node, rather than relying on debian package.  This is needed to support building dynamic substitutions.

## [1.1] - 2025-06-28

### Added

- Install system level `cairo` package for prefigure.

## [1.0] - 2025-06-16

Initial versioned release.

- Working versions of `oscarlevin/pretext` and `oscarlevin/pretext-full` replaced the previous tagged `oscarlevin/pretext:lite`, `oscarlevin/pretext:small`, and `oscarlevin/pretext:full`.
- Tags for `:latest` and `:1.0` created.
- GitHub action for build/deploy will read version from `version.txt` which will be updated manually
