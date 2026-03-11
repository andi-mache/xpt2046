# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.4.0] - 2026-03-11

### Changed

- **Breaking:** Migrated from `embedded-hal 1.0.0-alpha.7` to `embedded-hal 1.0.0` (stable).
- **Breaking:** `SPI` bound changed from `embedded_hal::spi::blocking::Transfer<u8>` to
  `embedded_hal::spi::SpiBus<u8>`. The chip-select pin (`CS`) is still managed manually by
  the driver; use `SpiBus` (not `SpiDevice`) when constructing your SPI peripheral.
- **Breaking:** `CS` bound changed from `embedded_hal::digital::blocking::OutputPin` to
  `embedded_hal::digital::OutputPin` (the `blocking` sub-module was removed in stable).
- **Breaking:** `DelayUs` bound on `init()` and `calibrate()` changed to
  `embedded_hal::delay::DelayNs`. `DelayNs::delay_ms()` is now infallible (returns `()`),
  so call sites no longer propagate a delay error.
- Updated `embedded-graphics` dependency from `0.7.1` to `0.8.0`.
- Updated `embedded-graphics-core` dependency from `0.3.3` to `0.4.0`.
- `Drawable` import in `calibration.rs` updated to `embedded_graphics::prelude::Drawable`
  to match the path change in `embedded-graphics 0.8`.
- Updated `stm32f4xx-hal` dev-dependency from `0.13.0` to `0.14.0`.

### Added

- `spi_read()` now calls `SpiBus::flush()` before deasserting chip-select, satisfying the
  `SpiBus` contract that guarantees all bytes have been clocked out before CS is released.
- Compatibility table added to `README.md` mapping crate versions to `embedded-hal` and
  `embedded-graphics` versions.

### Fixed

- Typo "Embeddd" corrected to "embedded" in `README.md`.
- `examples/rtic.rs`: `xpt_drv.init()` result is now properly handled with `.expect()`
  instead of being silently discarded.
- `examples/rtic.rs`: `xpt.run()` result in the idle loop now calls `.ok()` to explicitly
  acknowledge and discard the error, satisfying `#[must_use]`.
- `examples/rtic.rs`: Updated to `stm32f4xx-hal 0.14` API — delay now uses
  `cp.SYST.delay(&clocks)` returning `SysDelay` instead of the timer-peripheral-based
  `Delay<TIM1, 1000000>`.

## [0.3.1] - (previous release)

### Notes

- Last release targeting `embedded-hal 1.0.0-alpha.7` and `embedded-graphics 0.7.x`.
- See the repository history for changes prior to 0.4.0.
