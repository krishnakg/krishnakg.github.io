+++
title = "Using Claude Code with Gemini/OpenAI models"
author = ["Krishna Gopalakrishnan"]
date = 2025-04-18T22:09:00-07:00
lastmod = 2025-04-18T22:11:12-07:00
tags = ["gemini", "claude", "ai"]
draft = false
+++

I've been wanting to try Anthropic's Claude Code for a while now, and tried this out today. While exploring options, I came across [Claude Code Proxy](https://github.com/1rgs/claude-code-proxy) which is a neat way to use Gemini or OpenAI models with Claude code, instead of just relying on the (expensive) Anthropic models. It works by proxying prompts from claude models to Gemini/OpenAI models that you can configure. To save cost, it uses a BIG_MODEL for complex queries, and SMALL_MODEL for simpler queries. Sonnet and Haiku are the default models for Anthropic, but I tried with `gemini-2.5-pro-preview-03-25` for the BIG_MODEL, and `gemini-2.0-flash` for the SMALL_MODEL. Use the [Gemini models](https://ai.google.dev/gemini-api/docs/models) page for up-to-date names of these models.

<!--more-->

Steps to set this up were pretty straight forward. Cloned the Claude Code Proxy repository, set up the .env file with the right models to use, and started the proxy server. I just followed the instructions on the repository without any tweaks. Once the server is up and running, start claude code in the project directory.

```shell
# Point Claude Code at the local proxy server.
ANTHROPIC_BASE_URL=http://localhost:8082 claude
```

I tested this setup on a Rust project I recently started. Overall, it worked quite well. The quality of generated boilerplate code, for instance, was excellent.

However, it wasn't without hiccups. Complex refactoring prompts sometimes required multiple attempts. In one case, the model started applying changes, but then seemed to get confused by the file's modified state as it continued, requiring correction. Another issue occurred when a prompt errored out mid-execution; retrying sometimes led to duplicate code being inserted. Interestingly, the model often managed to identify and fix such duplication in subsequent prompts.

In spite of these occasional issues, I am genuinely impressed with what these latest models and tools can achieve. I can see myself regularly using this setup for generating well-defined functions and assisting with refactoring tasks. The productivity boost compared to coding entirely manually is definitely noticeable.
