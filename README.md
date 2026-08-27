# logdog

A better `adb logcat` for the terminal: colors and filtering like Android Studio's Logcat panel.

Single-file Python 3 script, zero dependencies. Requires `adb` in PATH.

## Install

```bash
ln -s "$PWD/logdog" /usr/local/bin/logdog   # or copy anywhere in PATH
```

## Usage

```bash
logdog com.myapp.debug              # only logs from this package (tracks process restarts)
logdog "com.myapp*"                 # prefix match (all build variants / :remote processes)
logdog --current                    # current foreground app
logdog -t MyTag -t "Retrofit.*"     # filter by tag (regex, repeatable)
logdog -i chatty -i ViewRootImpl    # hide noisy tags
logdog -l W                         # warnings and above
logdog com.myapp -g "auth token"    # grep messages (match highlighted)
logdog --time --show-pid            # extra columns
logdog -b crash --dump              # dump crash buffer and exit
logdog -s emulator-5554 com.myapp   # pick device
logdog --clear com.myapp            # clear buffer first
```

## Features

- **Package filtering by PID** — resolves running PIDs on start, then follows
  `ActivityManager` start/death events live, so logs survive app restarts
  (same mechanism Android Studio uses). Process start/death shown as `✦` banners.
- **Android Studio-style colors** — per-level colored badge and message
  (V gray, D blue, I green, W orange, E red, F magenta), stable per-tag colors.
- **Tag dedup** — consecutive identical tags shown once (disable with `--always-tags`).
- **Message wrapping** aligned under the tag column.
- Level threshold, tag include/exclude regex, message grep with highlight,
  multiple buffers, device selection, dump mode.
