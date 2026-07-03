# 02MaxMin

Small Java Maven project used to validate [TestNexus](https://github.com/Yousef-karem/Unit-test-demo) incremental test generation on push.

## Project

- **Source:** `src/main/java/ds/MaxMin1.java` — finds max and min in an integer array
- **Manual test:** `src/test/java/ds/MaxMin1Test12.java`

## TestNexus CI

On push or pull request that changes `.java` files, the GitHub Actions workflow (`.github/workflows/testnexus.yml`):

1. Detects changed/deleted Java files via git diff
2. Prunes stale `LLM_Generated*Test` classes for those sources
3. Regenerates tests incrementally for changed code
4. Uploads `demo_out/` as a workflow artifact

### Self-hosted runner required

The workflow uses `runs-on: [self-hosted, linux]`. If you see **"Waiting for a runner to pick up this job"**, no runner is online — the job is queued, not stuck.

**One-time setup on your Linux machine:**

1. Open [github.com/Yousef-karem/02MaxMin/settings/actions/runners](https://github.com/Yousef-karem/02MaxMin/settings/actions/runners)
2. Click **New self-hosted runner** → **Linux**
3. Run the download/configure commands GitHub shows, for example:

```bash
mkdir -p ~/actions-runner && cd ~/actions-runner
curl -o actions-runner-linux-x64-2.322.0.tar.gz -L \
  https://github.com/actions/runner/releases/download/v2.322.0/actions-runner-linux-x64-2.322.0.tar.gz
tar xzf ./actions-runner-linux-x64-2.322.0.tar.gz
./config.sh --url https://github.com/Yousef-karem/02MaxMin --token <TOKEN_FROM_GITHUB>
./run.sh
```

4. Keep `./run.sh` running (or install as a service with `sudo ./svc.sh install`)

**Runner prerequisites:** Python 3.10+, Java, Maven, Docker, and Ollama (`ollama serve`).

Until a runner is online, cancel queued runs from the Actions tab — they will not start on their own.

## Local incremental run

From a clone of [Unit-test-demo](https://github.com/Yousef-karem/Unit-test-demo):

```bash
python scripts/testnexus_ci.py \
  --repo /path/to/02MaxMin \
  --tool-script llm_coverage_demo.py \
  --output-dir /path/to/02MaxMin/demo_out \
  --analysis-diff-base HEAD~1 \
  --docker-maven
```
