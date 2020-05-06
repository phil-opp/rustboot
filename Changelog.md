# 0.9.2

- **Nightly Breakage:** Use `llvm_asm!` instead of deprecated `asm!` ([#108](https://github.com/rust-osdev/bootloader/pull/108))

# 0.9.1

- SSE feature: remove inline assembly + don't set reserved bits ([#105](https://github.com/rust-osdev/bootloader/pull/105))

# 0.9.0

- **Breaking**: Identity-map complete vga region (0xa0000 to 0xc0000) ([#104](https://github.com/rust-osdev/bootloader/pull/104))

# 0.8.9

- Implement boot-info-address ([#101](https://github.com/rust-osdev/bootloader/pull/101))

# 0.8.8

- Add basic support for ELF thread local storage segments ([#96](https://github.com/rust-osdev/bootloader/pull/96))

# 0.8.7

- Fix docs.rs build (see commit 01671dbe449b85b3c0ea73c5796cc8f9661585ee)

# 0.8.6

- Objcopy replaces `.` chars with `_` chars ([#94](https://github.com/rust-osdev/bootloader/pull/94))

# 0.8.5

- Update x86_64 dependency ([#92](https://github.com/rust-osdev/bootloader/pull/92))

# 0.8.4

- Move architecture checks from build script into lib.rs ([#91](https://github.com/rust-osdev/bootloader/pull/91))

# 0.8.3

- Remove unnecessary `extern C` on panic handler to fix not-ffi-safe warning ([#85](https://github.com/rust-osdev/bootloader/pull/85))

# 0.8.2

- Change the way the kernel entry point is called to honor alignement ABI ([#81](https://github.com/rust-osdev/bootloader/pull/81))

# 0.8.1

- Add a Cargo Feature for Enabling SSE ([#77](https://github.com/rust-osdev/bootloader/pull/77))

# 0.8.0

- **Breaking**: Parse bootloader configuration from kernel's Cargo.toml ([#73](https://github.com/rust-osdev/bootloader/pull/73))
    - At least version 0.7.7 of `bootimage` is required now.
- Configurable kernel stack size, better non-x86_64 errors ([#72](https://github.com/rust-osdev/bootloader/pull/72))
- Dynamically map kernel stack, boot info, physical memory and recursive table ([#71](https://github.com/rust-osdev/bootloader/pull/71))

# 0.7.1

- Run cargo update (improves compile times because of trimmed down upstream dependencies)

# 0.7.0

- **Breaking**: Only include dependencies when `binary` feature is enabled ([#68](https://github.com/rust-osdev/bootloader/pull/68))
    - For manual builds, the `binary` feature must be enabled when building
    - For builds using `bootimage`, at least version 0.7.6 of `bootimage` is required now.

# 0.6.4

- Use volatile accesses in VGA code and make font dependency optional ([#67](https://github.com/rust-osdev/bootloader/pull/67))
  - Making the dependency optional should improve compile times when the VGA text mode is used.

# 0.6.3

- Update CI badge, use latest version of x86_64 crate and rustfmt ([#63](https://github.com/rust-osdev/bootloader/pull/63))

# 0.6.2

- Remove stabilized publish-lockfile feature ([#62](https://github.com/rust-osdev/bootloader/pull/62))

# 0.6.1

- Make the physical memory offset configurable through a `BOOTLOADER_PHYSICAL_MEMORY_OFFSET` environment variable ([#58](https://github.com/rust-osdev/bootloader/pull/58)).
- Use a stripped copy of the kernel binary (debug info removed) to reduce load times ([#59](https://github.com/rust-osdev/bootloader/pull/59)).

# 0.6.0

- **Breaking**: Don't set the `#[cfg(not(test))]` attribute for the entry point function in the `entry_point` macro
    - With custom test frameworks, it's possible to use the normal entry point also in test environments
    - To get the old behavior, you can add the `#[cfg(not(test))]` attribute to the `entry_point` invocation
- Additional assertions for the passed `KERNEL` executable
    - check that the executable exists (for better error messages)
    - check that the executable has a non-empty text section (an empty text section occurs when no entry point is set)

# 0.5.3

- Mention minimal required bootimage version in error message when `KERNEL` environment variable is not set.

# 0.5.2

- Remove redundant import that caused a warning

# 0.5.1

- Add a `package.metadata.bootloader.target` key to the Cargo.toml that can be used by tools such as `bootimage`.

# 0.5.0

- **Breaking**: Change the build system: Use a build script that expects a `KERNEL` environment variable instead of using a separate `builder` executable as before. See [#51](https://github.com/rust-osdev/bootloader/pull/51) and [#53](https://github.com/rust-osdev/bootloader/pull/53) for more information.
  - This makes the bootloader incompatible with versions `0.6.*` and earlier of the `bootimage` tool.
  - The bootloader also requires the `llvm-tools-preview` rustup component now.

# 0.4.0

## Breaking

- The level 4 page table is only recursively mapped if the `recursive_page_table` feature is enabled.
- Rename `BootInfo::p4_table_addr` to `BootInfo::recursive_page_table_addr` (only present if the cargo feature is enabled)
- Remove `From<PhysFrameRange>` implemenations for x86_64 `FrameRange`
  - This only works when the versions align, so it is not a good general solution.
- Remove unimplemented `BootInfo::package` field.
- Make `BootInfo` non-exhaustive so that we can add additional fields later.

## Other

- Add a `map_physical_memory` feature that maps the complete physical memory to the virtual address space at `BootInfo::physical_memory_offset`.
- Re-export `BootInfo` at the root.
