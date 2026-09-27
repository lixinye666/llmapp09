# Free Ollama model fallback implementation plan

> **For the implementer:** Apply these steps in order, then run the listed verification commands before pushing.

1. Update runtime defaults
   - In `docker-compose.yml`, set the sentiment, summarization, and intent fallback values to `gemma4:31b`.
   - In `llm-multiroute/app/config.py`, make the corresponding application-setting defaults match.
   - In `llm-multiroute/k8s/deployment.yaml`, make the ConfigMap values match.

2. Update configuration assertions
   - In `llm-multiroute/tests/test_ai_service.py`, expect `gemma4:31b` for each task's selected model.
   - In `llm-multiroute/tests/test_ai_controller.py` and `llm-multiroute/tests/test_model_router.py`, update route-map expectations to the new defaults.

3. Correct the structured sentiment relevance metric
   - In `deepeval-tests/test_sentiment.py`, replace the generic answer-relevancy metric with a sentiment-specific GEval rubric.
   - Retain the existing score threshold and require that the response's sentiment and emotions relate to the input, while explicitly excluding a requirement to echo the input's subject matter.

4. Verify locally and remotely
   - Run `pytest tests/ -q --tb=short` from the backend directory.
   - Review the diff, commit and push to `main`.
   - Rerun DeepEval and PromptFoo on GitHub Actions and monitor both until completion.
