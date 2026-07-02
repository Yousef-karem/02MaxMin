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

Requires a **self-hosted runner** with Python, Java, Maven, Docker, and Ollama.

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
