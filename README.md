# Logbook

Records what Current You do so that Future You does not get angry at Past You

## Synopsis

Logbook is a simple POSIX compliant shell script that makes it easy to capture timestamped logs of your work.
It is essentially a glorified `echo "message" >> log.txt` that makes date handling invisible.
The author wanted to have a tool that records events with the minimum amount of cognitive load on his part.
Logbook uses a symlink in the user's home directory (`~/logbook.md`) pointing to the currently open logbook.
This allows any tool or editor to access the active logbook from any terminal without configuration (e.g. `nvim ~/logbook.md` or `glow ~/logbook.md`).

## Templates

A logbook is created from a template. By default this is a simple markdown file that adds the title and time created to the header.
You can create new templates to fit the kind of projects you work on.
For computer repair, you may want to add `@CUSTOMER@`, `@ASSET_TAG@`, and `@DEADLINE@` to the template.
When creating a new logbook you will be asked to fill these values in. Future You will thank you.
These names are reserved and will be replaced without warning: `@CREATION_TIME@` and `@LOGBOOK_PATH@`

## Installation

Make `logbook` executable and move it into your `$PATH`: `chmod +x logbook && mv logbook ~/.local/bin/`.
Also consider adding `alias log=logbook log` to your shell profile to minimize typing.

## Paths

- Project directory `./logbook/` stores all files related to a single project
- Project logbook `./logbook/logbook.md` is the log all events get appended to
- Application directory `~/.logbook/` stores application wide settings
- Logbook template `~/.logbook/template.md` is copied to new logbooks with variables substituted
- Open project `~/.logbook/current/` is a symlink to the active project directory
- Open logbook `~/logbook.md` is a symlink to the active project's `logbook.md`
- Logbooks index `~/.logbook/logbooks.md` contains links to all created project logbooks

## Examples

`logbook new [template]` 
- creates a new project directory
- clones the logbook template `~/.logbook/[template].md` with 2-pass variable substitution:
  1. Replaces reserved automatic variables (`@CREATION_TIME@`, `@LOGBOOK_PATH@`)
  2. Interactively prompts for any remaining `@PLACEHOLDER@` variables found in the template
- Calls `logbook open`
- Appends logbook path as link in logbooks index
Defaults to `~/.logbook/template.md`. A minimal default template is created if it does not exist.
Prints an error if the specified template file does not exist.

`logbook open` makes a symlink from ~/.logbook/current to ./logbook, effectively opening the notebook of a project folder.
Prints an error if a logbook directory does not exist.

`logbook log message` silently appends "${datetime}: ${message}" to the open project logbook.
Multi-line messages are logged as "${datetime}\n\n${message}" instead.

`logbook log` logs stdin to the currently open logbook, so messages can be piped to the logbook.
Prints an error if stdin is an interactive terminal.

`logbook run command` logs a command. The output is teed to the logfile.
- start: appends "${datetime}: Running ${command}" so that the start time is captured.
- finish: appends a collapsible code block with "${datetime}: finished with code x" as the summary, and the commmand output as the detail.
  Leave a blank line before the backticks inside <details>, otherwise standard Markdown parsers will not render the code block inside HTML details elements.

`logbook` prints usage information and the path to the open logbook, or a note if no logbook is open.