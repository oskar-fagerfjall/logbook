# Logbook

Records what Current You do so that Future You does not get angry at Past You

## Synopsis

Logbook is a simple POSIX-compliant shell script that makes it easy to capture timestamped logs of your work.
It is essentially a glorified `echo "message" >> log.txt` that makes date handling invisible.
The author wanted a tool that records events with the minimum amount of cognitive load on his part.

Logbook tracks open logbooks per terminal tab/window using session files in `~/.logbook/sessions/`. This allows different terminals to log to different files simultaneously as a standalone executable—without requiring `eval` wrappers or shell profile (`~/.bashrc`) modifications.

## Installation

Make `logbook` executable and move it into your `$PATH`:
```sh
chmod +x logbook && mv logbook ~/.local/bin/
```
Also consider adding `alias L="logbook log"` to your shell profile to minimize typing.

## Paths

- **Default logbook (`~/logbook.md`)**: The default logbook path, listed at the top of the menu when running `logbook open`.
- **Application directory (`~/.logbook/`)**: Stores application-wide settings and session state.
- **Recents list (`~/.logbook/recent`)**: Stores paths to recently opened logbooks, one per line.
- **Sessions directory (`~/.logbook/sessions/`)**: Stores per-terminal session files mapping TTYs and session leader PIDs to active logbook paths.

## Examples

`logbook open $path`
Opens `$path` for the current terminal tab/window and bubbles `$path` to the top of the Recents list.
If `$path` does not exist, `logbook` creates the file automatically.

`logbook open`
Prints a numbered Recents list allowing the user to select a logbook for the current terminal session by typing its number.
The default `~/logbook.md` is always at the top of the list (selectable by pressing Enter).

`logbook log message`
Silently appends `> ${datetime}\n\n${message}\n\n` to the open logbook for the current terminal session.
Prints an error if no logbook is open in the terminal.

`logbook log`
Logs `stdin` to the open logbook for the current terminal session, allowing messages or command outputs to be piped directly (e.g. `echo "done" | logbook log`).
If called from an interactive shell, the program will block until you type `Ctrl+D` to stop the log. Think of this as "Diary Mode".
Prints an error if no logbook is open in the terminal.

`logbook run command`
Executes `command`, teeing output to stdout and capturing it in the open logbook for the current terminal session.
Prints an error if no logbook is open in the terminal.
Log format: `> ${datetime}\n\nCommand \`${command}\` exit code {x} in {y} seconds\n\n\`\`\`${output}\`\`\`\n\n`

`logbook`
Prints usage information and the path to the open logbook for the current terminal session, or a note if no logbook is open.