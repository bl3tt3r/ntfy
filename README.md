# Ntfy

[![crates.io](https://img.shields.io/crates/v/ntfy.svg)](https://crates.io/crates/ntfy)
[![Documentation](https://docs.rs/ntfy/badge.svg)](https://docs.rs/ntfy)
[![MIT](https://img.shields.io/crates/l/ntfy.svg)](LICENSE)

## Description

[Rust](https://rust-lang.org) library for sending notifications over [ntfy](https://ntfy.sh).

## Example

```rust,no_run
use ntfy::prelude::*;

#[tokio::main]
async fn main() -> Result<(), Error> {
    let dispatcher = dispatcher::builder("https://ntfy.sh")
        .credentials(Auth::credentials("username", "password")) // Add optional credentials
        //.proxy("socks5h://127.0.0.1:9050") // Add optional proxy (requires the `socks` feature)
        .build_async()?; // Build the dispatcher

    let action = Action::new(
        ActionType::Http,
        "Turn down",
        Url::parse("https://api.nest.com")?,
    );

    let payload = Payload::new("mytopic")
        .message("Hello, **World**!") // Add optional message
        .title("Alert") // Add optional title
        .tags(["warning"]) // Add optional tags
        .priority(Priority::High) // Set priority
        .actions([action]) // Add optional actions
        .click(Url::parse("https://example.com")?) // Add optional clickable URL
        .attach(Url::parse("https://example.com/file.jpg")?) // Add optional URL attachment
        .delay("30min") // Add optional delivery delay
        .markdown(true); // Enable Markdown formatting

    dispatcher.send(&payload).await?;

    Ok(())
}
```

More examples can be found in the [examples](examples) directory.

## Crate Feature Flags

The following crate feature flags are available:

| Feature               | Default | Description                                         |
|-----------------------|:-------:|-----------------------------------------------------|
| `async`               |   Yes   | Enables the async `dispatcher` and `subscriber`     |
| `blocking`            |   No    | Enables the blocking `dispatcher` and `subscriber`  |
| `async-dispatcher`    |   Yes   | Enables the async `dispatcher`                      |
| `blocking-dispatcher` |   No    | Enables the blocking `dispatcher`                   |
| `async-subscriber`    |   Yes   | Enables the async `subscriber`                      |
| `blocking-subscriber` |   No    | Enables the blocking `subscriber`                   |
| `socks`               |   No    | Enables SOCKS proxy support                         |
| `rustls`              |   Yes   | Enables TLS via `rustls`                            |
| `native-tls`          |   No    | Enables TLS via the platform's native implementation (OpenSSL on Linux) |

## Minimum Supported Rust Version (MSRV)

This project uses the Rust `2024` edition and requires a minimum compiler version of `1.85.0`.

## License

This project is distributed under the MIT license. See the [LICENSE](LICENSE) file for details.
