# CIAM Plugin Marketplace

A Claude Code marketplace for distributing reusable plugins that support
Customer Identity and Access Management (CIAM) engineering.

## Purpose

This repository provides a discoverable catalog of CIAM development plugins.
It includes the packaged `ciam-test-data` plugin and makes it installable
from GitHub without requiring users to know the original development path.

## Repository relationship

There are two separate repositories:

### 1. Plugin development repository

Repository: [ciam-test-data-plugin](https://github.com/sanjivsingh79/ciam-test-data-plugin)

Local development directory:

`C:\Users\sanji\claude-ciam-sdlc-plugin`

This repository is the source used to develop, maintain and version the
`ciam-test-data` plugin.

### 2. Marketplace repository

Repository: [ciam-plugin-marketplace](https://github.com/sanjivsingh79/ciam-plugin-marketplace)

This repository contains:
- `.claude-plugin/marketplace.json` — marketplace catalog.
- `plugins/ciam-test-data/` — packaged plugin distributed through the catalog.
- `README.md` — installation and architecture documentation.

The marketplace catalog references the packaged plugin using the relative
path `./plugins/ciam-test-data`. It does not depend on a developer's local
Windows directory.

When the source plugin changes, update the packaged copy in this repository
and commit the change so marketplace users receive the updated version.

## Plugin contents

The packaged plugin includes the following components:

- `.claude-plugin/plugin.json` — plugin identity and metadata.
- `.mcp.json` — MCP server configuration.
- `agents/test-data-agent.md` — instructions for the test-data agent.
- `skills/test-data-selection/SKILL.md` — reusable test-data selection skill.

The agent and skill help Claude Code work with CIAM test-data scenarios.
The configured MCP integration provides access to the supporting test-data
capabilities.

## Architecture

```text
Developer
   |
   v
Claude Code
   |
   v
ciam-test-data plugin
   |
   +--> Test-data agent
   |
   +--> Test-data selection skill
   |
   +--> MCP server configuration
             |
             v
       CIAM test-data tools
             |
             +--> Synthetic test-data file
             |
             +--> Synthetic CIAM service
```

In the local POC configuration, the MCP server uses `127.0.0.1:9000` and
the synthetic CIAM service uses `127.0.0.1:8090`. These are local development
endpoints, not public hosted services. Verify the configured server commands
and required services before using the plugin on another machine.

## Installation

Install Claude Code and open a Claude Code session.

### 1. Register this marketplace

Run the following command inside Claude Code:

```text
/plugin marketplace add sanjivsingh79/ciam-plugin-marketplace
```

### 2. Install the plugin

```text
/plugin install ciam-test-data@ciam-marketplace
```

### 3. Verify installation

```text
/plugin
```

Check the installed plugins list for `ciam-test-data`. Depending on the
Claude Code version, you may need to restart the session for changes to
take effect.

### 4. Update the plugin later

Refresh marketplace metadata:

```text
/plugin marketplace update ciam-marketplace
```

Then use `/plugin` to inspect the plugin and update it if an update is
available. Restart Claude Code if requested.

## Local development

The marketplace catalog points to the packaged plugin directory:

`plugins/ciam-test-data/`

To develop the plugin, work in the original plugin development repository.
After making and testing changes, copy the required plugin files into the
marketplace package and commit the updated package and catalog as appropriate.

Keep the plugin metadata, MCP configuration, agent and skill synchronized
between the source repository and the packaged copy.

## Important notes

- Use synthetic test data for development and demonstrations.
- Do not commit credentials, access tokens, private keys or real customer data.
- The MCP server and synthetic CIAM service must be configured and running
  as required by the plugin; marketplace installation does not automatically
  deploy those services.
- A marketplace catalog entry makes a plugin discoverable. It does not by
  itself guarantee that all external dependencies are installed or running.

## License

Add a license before redistributing this project if you intend to grant
others explicit reuse rights.
