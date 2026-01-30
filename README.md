# system-fonts

System font discovery and locale-based preset selection.

This crate provides a small API to:

- Detect the system locale
- Map locale strings to a `FontRegion`
- Resolve a prioritized list of installed system fonts using `fontdb`

It is designed to be consumed by UI toolkits (e.g. `egui_system_fonts`), where the caller loads
font bytes and registers them into the UI font system.

## Installation

```toml
[dependencies]
system-fonts = "0.1"
```

## Usage

### Detect region from locale

```rust
use system_fonts::{region_from_locale, FontRegion};

assert_eq!(region_from_locale("ko_KR.UTF-8"), FontRegion::Korean);
assert_eq!(region_from_locale("zh-Hant-TW"), FontRegion::TraditionalChinese);
```

### Resolve fonts for a specific locale

```rust,no_run
use system_fonts::{find_for_locale, FontStyle};

let (region, fonts) = find_for_locale("ko-KR", FontStyle::Sans);
println!("region={region:?}, fonts={}", fonts.len());
```

### Resolve fonts for the current system locale

```rust,no_run
use system_fonts::{find_for_system_locale, FontStyle};

let (_locale, region, fonts) = find_for_system_locale(FontStyle::Sans);
println!("region={region:?}, fonts={}", fonts.len());
```

## Notes

- Font resolution is best-effort: missing families are skipped.
- The internal font database is cached (loaded once per process).
- `FoundFont::key` is unique within a single run; do not persist it across runs.

## License

MIT OR Apache-2.0
