# Change Log

## 0.3.7


### Bug Fixes
- Fixed SHA-256 verification to properly handle all platform VSIXs (darwin-arm64, darwin-x64, linux-arm64, linux-x64, win32-x64)
- Store and verify platform-specific hashes instead of single hash for all platforms

## 0.3.3


### Bug Fixes
- Fixed CHANGELOG fetch from private repo in marketplace publishing workflow
- Updated repository_dispatch payload to include changelog section

