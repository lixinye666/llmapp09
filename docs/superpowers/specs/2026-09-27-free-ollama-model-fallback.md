# Free Ollama model fallback

## Goal

Allow every evaluation route to run with the repository's configured Ollama Cloud key without paid-model credits.

## Confirmed approach

Keep the existing `classify`, `sentiment`, `summarize`, and `intent` routes and their prompts unchanged. Configure each route's default model as `gemma4:31b`, the model verified by the evaluation run to return HTTP 200.

## Configuration consistency

Use the same default in the local Docker Compose environment, application settings, and Kubernetes ConfigMap. Update unit-test expectations so they test the intended routing configuration rather than stale paid model names.

## Verification

Run the backend test suite, push the change to `main`, rerun the failed DeepEval and PromptFoo workflows, and monitor their GitHub Actions results.

## Sentiment evaluation relevance

The generic answer-relevancy rubric incorrectly requires a structured sentiment response to repeat the input's topic. Use a sentiment-specific relevance rubric instead: it must assess the input's sentiment and emotional tone, but does not need to restate subjects, entities, or logistical details. This preserves the response contract and the schema, correctness, and emotion-accuracy checks.
