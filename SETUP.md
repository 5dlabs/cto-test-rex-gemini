# Gemini 3 CLI Test Setup

This repository is configured to test Google's **Gemini 3 Pro** (`gemini-3-pro-preview`) with the CTO platform.

## Configuration

The test is configured in `cto-config.json`:
- **CLI**: `gemini` (`@google/gemini-cli`)
- **Model**: `gemini-3-pro-preview` (Gemini 3 Pro Preview)
- **Repository**: `5dlabs/cto-test-rex-gemini`
- **Service**: `cto-test-rex-gemini`

## Prerequisites

1. **Google API Key**: Must be set in the `agent-platform` namespace
   ```bash
   kubectl get secret agent-platform-secrets -n agent-platform -o jsonpath='{.data.GOOGLE_API_KEY}' | base64 -d
   ```

2. **Gemini CLI**: Will be installed automatically via npm in the agent container:
   ```bash
   npm install -g @google/gemini-cli
   ```

## Running the Test

### Option 1: Via CTO MCP Tool
```bash
mcp_cto_play --task-id=1
```

### Option 2: Via Command Line
```bash
cd ~/code/work-projects/5dlabs/cto-test-rex-gemini
# Your cto-config.json should already be configured for Gemini 3
# Run whatever test command you use locally
```

## Task Details

**Task 1**: Simple integration test
- Creates a JavaScript file that prints "Hello from Gemini 3!"
- Tests basic CLI functionality
- Verifies Gemini 3 API key and model access

## Verification

Check the workflow status:
```bash
kubectl get workflows -n agent-platform -l service=cto-test-rex-gemini
kubectl get coderun -n agent-platform -l service=cto-test-rex-gemini
```

Watch logs:
```bash
kubectl logs -n agent-platform -l service=cto-test-rex-gemini -f
```

## Available Gemini Models

Query Google AI for available models:
```bash
curl -s "https://generativelanguage.googleapis.com/v1beta/models?key=$GOOGLE_API_KEY" | \
  jq -r '.models[] | select(.name | contains("gemini")) | .name'
```

Current Gemini 3 models:
- `models/gemini-3-pro-preview` ✅ (configured)

## Notes

- Gemini 3 requires API access (paid tier or waitlist approval)
- The model name in the API is prefixed with `models/` but we use `gemini-3-pro-preview` in configuration
- The Gemini CLI handles the `models/` prefix automatically
