# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2025-06-25

### Major Refactoring - Library + CLI Architecture

**BREAKING CHANGES:**
- Complete refactor from monolithic CLI to library + CLI architecture
- New public API for programmatic usage
- C FFI bindings for cross-language integration

### Added
- **🦀 Rust Library API** with `get()`, `get_stream()`, `get_with_progress()`, `get_with_options()`
- **📚 Static & Dynamic Libraries** for both Rust (`rlib`) and C-compatible (`a`, `so`, `dylib`, `dll`)
- **🔗 C FFI Bindings** with thread-safe progress callbacks and comprehensive C header
- **⚡ Smart Connection Strategy** - Single connection for files ≤1MB, scaled connections for larger files
- **🔧 pkg-config Support** for system-wide library installation
- **📊 Comprehensive Benchmarking** against curl and aria2 with MD5 validation
- **🏗️ Makefile Build System** with `make all`, `make c-lib`, `make install` targets
- **📝 Centralized Version Management** - Single `VERSION` file drives all components

### Performance Optimizations
- **Smart connection scaling** based on file size (1-16 connections)
- **Direct I/O support** for large files (>1GB) on Unix systems
- **Memory efficiency** - <1GB RAM usage regardless of file size
- **HTTP User-Agent versioning** for proper server identification

### Library Features
- **Progress callbacks** with `Arc<dyn Fn(u64, u64)>` for thread-safe progress tracking
- **Streaming API** for pipeline integration
- **Feature flags** for optional S3 support (`default = ["s3"]`)
- **Cross-platform support** - Windows, macOS, Linux

### Benchmarking & Testing
- **Multi-tool benchmarking** comparing butterfly-dl vs curl vs aria2
- **Automatic tool detection** - only tests available tools
- **Fair comparison** - matching connection strategies across tools
- **MD5 checksum validation** for file integrity verification
- **Performance metrics** - duration, speed, success/failure tracking

### Developer Experience
- **Comprehensive documentation** with usage examples for Rust and C
- **Build-time version management** - change `VERSION` file, rebuild gets new version everywhere
- **Example code** for both library and C FFI usage
- **Automated cleanup** in benchmark scripts

### Infrastructure
- **Build script integration** reads `VERSION` file and sets environment variables
- **Dependency tracking** - VERSION file changes trigger rebuilds
- **pkg-config template** for proper system integration

## [0.1.0] - 2025-06-24

### Added
- **Geofabrik PBF Downloader Component** - Initial working implementation
- **Multi-connection parallel downloads** with 8 connections and 100MB chunks for optimal performance
- **File freshness checking** - Skip downloads if files are newer than `RENEW_PBF_PERIOD` (default: 7 days)
- **Complete CLI interface** with country, continent, and batch download support
- **List command** with filtering (countries/continents/all)
- **Dry-run mode** for previewing downloads without downloading
- **FROM scratch production Docker image** (13.4MB) for minimal attack surface
- **Development Docker image** with full Alpine + Rust toolchain for debugging
- **Convention over configuration** approach with hardcoded optimal defaults
- **Comprehensive test coverage** (16 tests) including integration tests
- **Makefile with production/development workflows**
- **Environment variable configuration** for logging and renewal period

### Performance
- **8 parallel connections** hardcoded for maximum speed
- **100MB chunks** optimized for large file downloads
- **Automatic range request detection** with fallback to single connection
- **Progress tracking** with real-time connection count display
- **Speed improvements**: Up to 15-40 MB/s vs 2-5 MB/s single connection

### Infrastructure
- **Docker-first development** with both production and development images
- **XP pair programming** workflow with human + AI collaboration
- **Comprehensive documentation** with clear usage examples
- **MIT license** and proper project metadata

### Notes
- This is the first component of the larger **butterfly** project
- Implements a complete, production-ready Geofabrik downloader
- Foundation established for future components and integrations