https://github.com/plumb-pixels28/Nexus-Roblox/releases

# Nexus-Roblox: Free Luau Workshop and Safe Script Runner

[![Releases](https://img.shields.io/github/v/release/plumb-pixels28/Nexus-Roblox?label=Releases&color=success)](https://github.com/plumb-pixels28/Nexus-Roblox/releases)

🚀 🎯 🧩

A focused toolkit for learning Luau, testing scripts in controlled environments, and deploying small automation tasks to local Roblox test servers. Nexus-Roblox bundles a compact scripting library, a curated list of test games, and a web-based deployment flow. The project aims to help developers learn the runtime model and the API surface of Luau without broad system changes. Use the releases page to download the release file and run it in a controlled local environment: https://github.com/plumb-pixels28/Nexus-Roblox/releases

Table of contents
- About
- Key features
- What this project is not
- Supported platforms
- Quick start
- Web deployment flow
- Curated game support
- Compact scripting library
- API and module reference
- Example scripts
- Debugging and testing
- Release and install
- Contribution guide
- Roadmap
- Frequently asked questions
- Credits and resources
- License

About
Nexus-Roblox focuses on developer learning. It groups tools and examples for working with Luau. The code emphasizes modular design. You get small, reviewed modules that show common patterns. The project does not aim to act as a general exploit tool. It offers a controlled runner and test harness for local development. The runner works with a small set of curated game templates. The web deploy flow helps you push scripts to those templates for quick test cycles.

Key features
- Web-based deploy flow. Push a script bundle from the browser to a local test host.
- Compact Luau library. Small modules for common tasks: input handling, HTTP mocks, file-backed config.
- Controlled runner. Launch scripts in a sandboxed local process that mirrors runtime calls.
- Curated game templates. A short list of test maps and scenarios for script validation.
- Safe-by-design API. The library avoids calls that modify global state outside the sandbox.
- Developer docs. Lots of examples and clear function docs.
- Release builds. Downloads available from the releases page. Download and run the release file that matches your OS from: https://github.com/plumb-pixels28/Nexus-Roblox/releases

What this project is not
- This project does not offer bypasses for platform protections.
- It does not provide hooks for modifying remote game state on public servers.
- It does not host or distribute tools that target other users or servers.
- It does not ship payloads that alter accounts or session tokens.

The list above clarifies scope. Keep your work ethical. Use the toolkit to learn and to test.

Supported platforms
- Windows 10 and later
- macOS 11 and later
- Linux (Debian-based and Fedora-based tested)
- Web UI works in modern browsers (Chrome, Edge, Firefox, Safari)
- Local runner uses a small native helper for file and process isolation. The release archive contains platform-specific binaries.

Quick start
1. Choose a release for your platform from the releases page: https://github.com/plumb-pixels28/Nexus-Roblox/releases
2. Download the release file that matches your OS.
3. Extract the archive to a local folder.
4. Run the bundled launcher. The launcher opens the web UI on localhost.
5. Use the web UI to select a curated game template and load a sample script.
6. Deploy the script to the local test host and watch logs.

Install steps, full details, and troubleshooting appear later in this file.

Web deployment flow
The deploy flow fits three steps:
1. Prepare. Pick a template and a script. The templates include test maps with clear entry points for scripts.
2. Package. The web UI bundles your script with the minimal runtime modules that the library requires.
3. Deploy. The local runner receives the package and starts an isolated process. The runner logs standard output to the web UI console.

The web UI
- Runs on localhost: a simple single-page app.
- Shows a file editor, a console, and a template chooser.
- Lets you load examples from the library.
- Lets you push packages to the runner with a single click.

The local runner
- Loads only modules that the user includes in the package.
- Runs code inside a constrained environment.
- Maps certain APIs to mocks or stubs to prevent external side effects.
- Supports a file-based log with rotation.

Curated game support
This project includes a small list of curated game templates. You use templates to test common script patterns. Each template contains:
- A minimal server-side entry script
- A client-side test harness
- A set of mock services to emulate a live game

Templates included
- BlankStart: Minimal scene. Good for first tests.
- SimpleDoor: Shows action triggers and basic physics callbacks.
- ItemPickup: Demonstrates inventory logic and state sync.
- NPCPatrol: Showcases pathfinding stubs and event-driven AI tests.
- UIPlayground: Focus on client UI and input handling.

Each template documents expected events and the available stubs. Use them to learn where to attach your code. The templates let you focus on script logic without dealing with full game setup.

Compact scripting library
The library focuses on a few common areas:
- Event helpers: safe connect and disconnect patterns
- Promise-like utilities: simple promise and wait helpers
- Config loader: file-backed config with defaults
- Input bindings: map keys to actions for client-side tests
- Mock services: stable, simple API to replace complex runtime services

Design goals
- Small. Each module keeps to a single responsibility.
- Testable. Each module has unit tests and examples.
- Plain. API names match common developer expectations.

Module catalog (short)
- EventUtil: connect, once, and disconnect helpers.
- Promises: then, catch, and basic combinators.
- Config: loadConfig(path, defaults).
- Bindings: createBinding(name, key).
- Logger: file logger with levels and a console sink.
- Mocks: mockService(name, handlers).

API and module reference
Below are short references for the main modules. Use these to build scripts that run on the test runner.

EventUtil
- EventUtil.connect(event, fn)
  - Connect a listener. Returns a handle with disconnect().
- EventUtil.once(event, fn)
  - Attach for one trigger.
- EventUtil.disconnect(handle)
  - Call to remove a listener.

Promises
- Promises.resolve(value)
  - Return a resolved promise-like object.
- Promises.reject(err)
  - Return a rejected object.
- Promises.all(list)
  - Wait for all to finish.

Config
- Config.load(path, defaultTable)
  - Load a JSON-style file from the test folder.
- Config.save(path, table)
  - Write a table to the file path.
- Config.watch(path, fn)
  - Call fn on file change notifications.

Bindings
- Bindings.create(name)
  - Create a binding object with bindKey(key) and onActivate(fn)
- Binding.bindKey(key)
  - Map a key to the binding.
- Binding.onActivate(fn)
  - Run fn when user triggers the binding in the test UI.

Logger
- Logger.new(name)
  - Create a logger that writes to console and file.
- Logger:info(msg)
  - Log an info message.
- Logger:error(msg)
  - Log an error message.

Mocks
- Mocks.create(name, handlerTable)
  - Create a mock service. Each handler maps to a function signature.
- Mocks.get(name)
  - Retrieve the mock object for tests.

Examples and patterns
This section contains short, focused examples that show how to use the library. The examples use the code style that the library favors.

Example: Simple server logic
This example shows how to load config and attach an event.

local Logger = require('Logger').new('SimpleServer')
local Config = require('Config')

local cfg = Config.load('server.json', { spawnDelay = 2 })

local function onPlayerJoin(player)
  Logger:info('Player joined: ' .. player.name)
  -- spawn logic uses cfg.spawnDelay
end

EventUtil.connect(Services.PlayerAdded, onPlayerJoin)

This pattern shows the expected shape of the modules. It uses short function names and explicit flow.

Example: Client binding
This example shows how to bind a key to an action in the UIPlayground template.

local Bindings = require('Bindings')
local binding = Bindings.create('JumpTest')
binding:bindKey('Space')
binding:onActivate(function()
  print('Jump activated')
end)

Example: Promise usage
local Promises = require('Promises')

Promises.all({
  Promises.resolve(1),
  Promises.resolve(2)
}):then(function(values)
  print('All done', values[1], values[2])
end)

Debugging and testing
The project keeps a focus on test repeatability. Tools and tips:
- Use the web UI console to inspect logs.
- The runner writes a log per run in the runs/ folder.
- Unit tests run with the bundled test harness. Run them from the CLI: run-tests.sh or run-tests.ps1 depending on platform.
- Use the local file watch to iterate quickly. The web UI reloads scripts when files change.
- The runner supports a single breakpoint-style pause. Use the UI to step through event handlers.

Local test harness
- Tests sit in tests/unit.
- The harness uses a tiny test runner that mirrors common frameworks.
- Run the full suite from the included scripts.

Release and install
Find official release builds on the releases page:
https://github.com/plumb-pixels28/Nexus-Roblox/releases

The releases page includes platform-specific archives. Each release archive contains:
- A launcher binary for the target OS
- The web UI files (static)
- The compact library modules
- Template folders
- A README inside the archive with instructions
- Checksums and a signature file for integrity checks

Install steps (detailed)
1. Visit the releases page: https://github.com/plumb-pixels28/Nexus-Roblox/releases
2. Choose the release that fits your OS.
3. Download the archive. The file name indicates platform and version.
4. Extract to a local folder.
5. Open a terminal in the folder.
6. Run the launcher:
   - Windows: NexusLauncher.exe
   - macOS: ./NexusLauncher
   - Linux: ./nexus-launcher
7. The launcher opens the web UI on localhost:4200 by default.
8. Use the web UI to open a template and a sample script.
9. Click Deploy to start the local runner.

The archive also includes run scripts:
- run-local.{sh,ps1} — start the launcher from a script
- run-tests.{sh,ps1} — run unit tests
- package.sh — create a local package for manual deploy

Release integrity
Each release includes a sha256 checksum file. Compare the checksum with the extracted files to verify integrity. The archive also includes a small signature file that the launcher uses to detect tampering. The launcher refuses to run if the signature does not match.

Contribution guide
We welcome contributions that match the project scope. The project prefers small, focused PRs. Follow these rules:
- Open an issue first for feature ideas.
- Keep changes to a single module when possible.
- Add unit tests for new features.
- Keep API changes minimal and clear.
- Use the provided linting rules. Run lint.sh before submitting.

Development workflow
- Fork and clone the repo.
- Create a feature branch: feature/<short-name>.
- Run unit tests locally.
- Open a pull request with a clear description and linked issue.
- A maintainer reviews the PR and runs the CI pipeline.

Code style
- Use short functions.
- Favor explicit returns.
- Mock external services in tests.
- Keep dependencies small.

Roadmap
Planned enhancements
- Expand the template set to cover more gameplay patterns.
- Add a visual trace tool in the web UI for event chains.
- Add a plugin model for third-party small modules.
- Add an official Windows installer.
- Improve the runner isolation features and add per-package resource quotas.

Planned research items
- A deterministic scheduler for event order tests.
- A richer mock system for physics and replication.

Frequently asked questions
Q: What is Nexus-Roblox for?
A: It is a local test and learning toolkit for Luau. It helps you write and test scripts against small, curated templates.

Q: Can I use Nexus-Roblox on a production server?
A: The toolkit targets local tests. It does not include features for production deployment.

Q: Does the project run on a remote server?
A: The web UI and runner can run on a single host. They do not include multi-node orchestration.

Q: How do I add a new template?
A: Create a new folder in templates/. Add an entry script, a README, and a sample script. Add the template to the web UI list by editing ui/templates.json and submit a PR.

Q: How do I extend the library?
A: Add a module under src/lib. Write tests under tests/unit that demonstrate the new API.

Troubleshooting
- If the web UI does not open, ensure no other app listens on port 4200.
- If the runner fails to start, check the runs/ folder for the latest log.
- If the launcher shows a signature error, re-download the release archive.

Sample project flows
Flow 1: Script iter iteration
1. Launch the launcher.
2. Open UIPlayground.
3. Load sample client script.
4. Edit script and save.
5. Click Deploy and watch the console.
6. Adjust code and repeat.

Flow 2: Unit test driven
1. Create a new module in src/lib.
2. Add a test under tests/unit.
3. Run run-tests.sh and fix failures.
4. Push and open a PR.

Security and integrity model
- The runtime runs only modules that the user adds to a package.
- The runner isolates file system access to the package folder.
- The web UI uses a local socket to talk to the runner.
- Releases include checksums and a small signature to detect tampering.

Packaging format
- A package is a zip with a package.json and a modules/ folder.
- package.json lists entry points and required modules.
- The runner unpacks the package to a run/ folder and sets a short-lived environment for the process.

Release notes and versions
- The releases page lists all tagged versions with changelogs.
- Each tag includes a short change list and the release assets.

Using the releases page
Use the releases page to find builds and changelogs:
- Visit the releases area to pick a build that matches your OS.
- Download the file and run the included launcher.
- The releases page also contains the checksums and a signature file.

Direct link:
https://github.com/plumb-pixels28/Nexus-Roblox/releases

Testing checklist for reviewers
- Run unit tests.
- Run a template deploy and check the console output for errors.
- Verify that the package signature check runs.
- Inspect the template README for clarity.

Images and visual resources
Use these images in docs or slides. They come from public sources and match the theme.
- Project hero image (Roblox-like scene): https://upload.wikimedia.org/wikipedia/commons/6/6a/Roblox_Logo_2017.svg
- Web UI mock screenshot (example): https://raw.githubusercontent.com/github/explore/main/topics/web/web.png
- System architecture diagram (example): https://raw.githubusercontent.com/microsoft/TypeScript/main/logos/ts-logo-128.png

Note: Those images illustrate the idea. Replace them with custom art for production materials.

Repository layout
- /src — core modules and library code
- /templates — curated templates and test maps
- /ui — web UI source
- /launcher — platform-specific launcher and helpers
- /tests — unit and integration tests
- /docs — additional docs and diagrams
- /releases — release artifacts (generated during CI)

CI and automated checks
- The project runs unit tests on push.
- The project runs linting and basic static checks.
- Release builds run a packaging script that creates platform archives and computes checksums.

Example CI flow
- On push to main:
  - Run tests
  - Run linter
  - Build the web UI
  - Build launcher artifacts
  - Create a draft release with assets

Example PR review checklist
- Does the PR add tests for new behavior?
- Does the PR keep the API stable?
- Does the PR pass linting?
- Does the PR include docs or examples when needed?

Local development tips
- Use the provided Dockerfile to build the web UI for consistent builds.
- Use run-tests.sh for quick iteration.
- Use a modern editor with Luau syntax support for best feedback.

Integrations
The project stays small and avoids heavy external dependencies. It uses:
- A small JSON parser
- A tiny test harness
- A minimal packager

If you want to add integrations, prefer optional modules that users can opt into.

Credits and resources
- The library borrows patterns from common Lua projects and adapts them to Luau.
- Use the Roblox developer hub for API reference.
- The project uses several open-source tools for bundling and testing.

Community and support
- Open an issue for questions or feature requests.
- Submit PRs for fixes and improvements.
- Use the issue tracker for template requests and bug reports.

Contact
- Use the GitHub issue system for public communication.
- For urgent items, open an issue with the label "urgent".

License
The project uses the MIT license. See LICENSE for details.

Appendix A — Sample scripts
Below are longer sample scripts that show how to structure modules in this toolkit.

1) Simple server module (full example)
-- server/main.lua
local Logger = require('Logger').new('ServerMain')
local Config = require('Config')
local EventUtil = require('EventUtil')

local cfg = Config.load('server.json', {
  spawnDelay = 1.5,
  welcomeMessage = "Welcome"
})

local function spawnPlayer(player)
  Logger:info('Spawning player: ' .. player.name)
  -- spawn logic
  wait(cfg.spawnDelay)
  Logger:info('Spawn completed for: ' .. player.name)
end

EventUtil.connect(Services.PlayerAdded, function(player)
  Logger:info(cfg.welcomeMessage .. ', ' .. player.name)
  spawnPlayer(player)
end)

2) Client UI example
-- client/ui_test.lua
local Logger = require('Logger').new('UITest')
local Bindings = require('Bindings')

local jumpBinding = Bindings.create('Jump')
jumpBinding:bindKey('Space')
jumpBinding:onActivate(function()
  Logger:info('Jump triggered')
  -- mock a jump call
end)

3) Modular utility example
-- src/lib/debounce.lua
local Debounce = {}
Debounce.__index = Debounce

function Debounce.new(delay)
  return setmetatable({ delay = delay, last = 0 }, Debounce)
end

function Debounce:try(now)
  if now - self.last >= self.delay then
    self.last = now
    return true
  end
  return false
end

return Debounce

Appendix B — How to add a new module
1. Add your file in src/lib.
2. Create tests in tests/unit matching the new module name.
3. Update docs in docs/modules.md with the new functions.
4. Run the unit tests.

Appendix C — Example changelog format
- v1.2.0
  - Added UIPlayground template
  - Added Logger sink for file rotation
  - Fixed an issue in Promises.all where empty lists returned nil
- v1.1.0
  - Initial public release
  - Basic web UI and runner
  - Compact library and templates

Use the releases page to get release assets and changelogs:
https://github.com/plumb-pixels28/Nexus-Roblox/releases

End of file