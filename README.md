# macos-init

This repo is an agent-readable macOS bootstrap spec.

`macos-config.md` defines the target setup for a new Mac:
- what to install
- what to configure
- how to verify

It is similar to a NixOS config in intent (target state), but implemented as a Markdown execution guide for agents.

## Usage

Give `macos-config.md` to an agent and let it execute the steps in order.
