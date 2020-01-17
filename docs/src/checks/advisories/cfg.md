# The `[advisories]` section

Contains all of the configuration for `cargo deny check advisories`

## Example Config

```ini
[advisories]
vulnerability = "deny"
unmaintained = "deny"
ignore = [
    # spin is unmaintained, but it has a couple of heavy users,
    # particularly lazy_static that will probably take a while
    # to get rid of
    "RUSTSEC-2019-0031",
]
```

### The `db-url` field (optional)

URL to the advisory database's git repo

Default: https://github.com/RustSec/advisory-db

### The `db-path` field (optional)

Path to the local copy of advisory database's git repo

Default: ~/.cargo/advisory-db

### The `vulnerability` field (optional)

Determines what happens when a crate with a security vulnerability is 
encountered.

* `deny` (default) - Will emit an error with details about each vulnerability, 
and fail the check.
* `warn` - Prints a warning for each vulnerability, but does not fail the check.
* `allow` - Prints a note about the security vulnerability, but does not 
fail the check.

### The `unmaintained` field (optional)

Determines what happens when a crate with an `unmaintained` advisory is 
encountered.

* `deny` - Will emit an error with details about the unmaintained advisory, and 
fail the check.
* `warn` (default) - Prints a warning for each unmaintained advisory, but does 
not fail the check.
* `allow` - Prints a note about the unmaintained advisory, but does not fail 
the check.

### The `notice` field (optional)

Determines what happens when a crate with a `notice` advisory is encountered.

**NOTE**: As of 2019-12-17 there are no `notice` advisories in 
https://github.com/RustSec/advisory-db

* `deny` - Will emit an error with details about the notice advisory, and fail 
the check.
* `warn` (default) - Prints a warning for each notice advisory, but does not 
fail the check.
* `allow` - Prints a note about the notice advisory, but does not fail the 
check.

### The `ignore` field (optional)

Every advisory in the advisory database contains a unique identifier, eg. 
`RUSTSEC-2019-0001`. Putting an identifier in this array will cause the 
advisory to be treated as a note, rather than a warning or error.

### The `severity-threshold` field (optional)

The threshold for security vulnerabilities to be turned into notes instead of 
warnings or errors, depending upon its
[CVSS](https://en.wikipedia.org/wiki/Common_Vulnerability_Scoring_System) score. 
So having a high threshold means some vulnerabilities might not fail the check, 
but having a log level `>= info` will mean that a note will be printed instead 
of a warning or error, depending on `[advisories.vulnerability]`.

* `None` (default) - CVSS Score 0.0
* `Low` - CVSS Score 0.1 - 3.9
* `Medium` - CVSS Score 4.0 - 6.9
* `High` - CVSS Score 7.0 - 8.9
* `Critical` - CVSS Score 9.0 - 10.0