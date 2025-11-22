# Change Log

All notable changes to the PreTeXt-Docker image will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

Instructions: Add a subsection under `[Unreleased]` for additions, fixes, changes, and removals of features accompanying each PR.

## [Unreleased]

### Added

- Add `aymptote` to tlmgr install list.
- Add `appendix`, `ncctools`, `threeparttable`, `sttools`, `wrapfig`, `multirow`, and `elsarticle` latex packages to support building elsevier articles.

### Changed

- Removed unneeded passagemath setup wheel building step.

## [1.4] - 2025-08-17

### Changed

- Use latest debian 13 (trixie) as base image.
- Added `polynom` latex package.

### Fixed

- Newer version of `python3-louis` allows for building braille targets without error.

## [1.3] - 2025-07-31

### Changed

- Switch to nodesource repository for node install.
- Update passagemath to 10.6.1.
- Add `parskip` and `marginnote` latex packages.

## [1.2] - 2025-07-10

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
