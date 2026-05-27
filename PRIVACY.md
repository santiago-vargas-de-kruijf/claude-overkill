# Privacy Policy

**Last updated:** 2026-05-27

## Overview

The `overkill` Claude Code plugin is a prompt-only skill. It contains no scripts, no executables, no network calls, and no external dependencies.

## Data collection

This plugin does **not** collect, transmit, store, or process any personal data or usage information. It has no access to:

- Your identity or account information
- Your code or file contents beyond what Claude already has in its context window
- Your network or system resources
- Any external services or APIs

## How it works

When invoked, the plugin provides instructions to Claude's language model about how to respond. All processing happens within your existing Claude Code session under Anthropic's standard privacy policy.

The optional `--current` flag instructs Claude to use its built-in web search and web fetch tools to verify references and check project health. Any network requests made in that mode are performed by Claude Code's own tooling under Anthropic's standard privacy policy — not by this plugin. No data is sent to any service controlled by this plugin.

## Third-party services

This plugin does not integrate with or send data to any third-party services.

## Contact

For questions, open an issue at https://github.com/santiago-vargas-de-kruijf/claude-overkill/issues
