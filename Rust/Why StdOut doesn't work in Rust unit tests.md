This happens because Rust test programs hide the stdout of successful tests in order for the test output to be tidy. You can disable this behavior by passing the `--nocapture` option to the test binary or to `cargo test`