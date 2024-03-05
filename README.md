# yxcvbn

[![License](https://img.shields.io/crates/l/zxcvbn.svg)](https://github.com/Identeco/yxcvbn/blob/master/LICENSE)

## Overview

`yxcvbn` is a German and English password strength estimator based on the rust crate [zxcvbn](https://github.com/shssoichiro/zxcvbn-rs), which is based on Dropbox's zxcvbn library.
using pattern matching and conservative estimation, it recognizes and weights

- 30k common passwords,
- German and English common names and surnames,
- popular German and English words from Wikipedia,
- and other common patterns such as dates, repeats (`aaa`), sequences (`abcd`), keyboard patterns (`qwertz`, `qwerty`, ...), and l33t speak.

Consider using yxcvbn as an algorithmic alternative to password composition policies — it is more secure, flexible, and usable when sites require a minimal complexity score instead of annoying rules like "passwords must contain three of {lower, upper, numbers, symbols}".

- __More secure__: Policies often fail both ways, allowing weak passwords (`P@ssword1`) and disallowing strong passwords.
- __More flexible__: yxcvbn allows many password styles to flourish as long as it detects sufficient complexity — passphrases are rated highly given enough uncommon words, keyboard patterns are ranked based on length and number of turns, and capitalization adds more complexity when it's unpredictable.
- __More usable__: yxcvbn is designed to power simple, rule-free interfaces that give instant feedback. In addition to strength estimation, yxcvbn includes minimal, targeted verbal feedback that can help guide users towards less guessable passwords.

## Installing

`yxcvbn` can be added to your project's `Cargo.toml` under the `[dependencies]` section, as such:

```toml
[dependencies]
yxcvbn = { git = https://github.com/Identeco/yxcvbn/ }
```

yxcvbn has a "ser" feature flag you can enable if you require serialization support via `serde`.
It is disabled by default to reduce bloat.

## Usage

`yxcvbn` exposes one function called `zxcvbn` which can be called to calculate a score (0-4) for a password as well as other relevant information.
`yxcvbn` may also take an array of user inputs (e.g. username, email address, city, state) to provide warnings for passwords containing such information.

Usage example:

```rust
extern crate yxcvbn;

use yxcvbn::zxcvbn;

fn main() {
    let estimate = zxcvbn("correcthorsebatterystaple", &[]).unwrap();
    println!("{}", estimate.score()); // 3

    let estimate = zxcvbn("Gartenhaus123", &[]).unwrap(); 
    println!("{}", estimate.score()); // 2
}
```

Other fields available on the returned `Entropy` struct may be viewed in the [full documentation](https://docs.rs/zxcvbn/*/zxcvbn/).

## Contributing

Any contributions are welcome and will be accepted via pull request on GitHub. Bug reports can be
filed via GitHub issues. Please include as many details as possible. If you have the capability
to submit a fix with the bug report, it is preferred that you do so via pull request,
however you do not need to be a Rust developer to contribute.
Other contributions (such as improving documentation or translations) are also welcome via GitHub.

## License

zxcvbn is open-source software, distributed under the MIT license.
