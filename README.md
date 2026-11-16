# hello-bot

A hello-world agent on [Norns](https://github.com/nornscode/norns). Good starting point for seeing how workers and clients work with the [Python SDK](https://github.com/nornscode/norns-sdk-python).

## Run

Start a local Norns server:

```sh
brew install nornscode/tap/nornsctl
nornsctl dev
```

The project depends on the SDK as an editable checkout (see `[tool.uv.sources]`
in `pyproject.toml`), so clone it next to this repo:

```sh
git clone https://github.com/nornscode/norns-sdk-python ../norns-sdk-python
```

Both the worker and the client read `NORNS_API_KEY` (and optionally `NORNS_URL`,
default `http://localhost:4001`); the worker also needs `ANTHROPIC_API_KEY`.
Export them, or put them in a `.env` file (`.envrc` loads it under direnv).

Start the worker:

```sh
uv sync
uv run hello-worker
```

In another terminal, send a message:

```sh
uv run hello-client
```

## What it demonstrates

The client says "Hello!" without giving a name, so the agent calls the built-in
`ask_human` tool and pauses:

```
Agent asks: What's your name?
> Anson

Run 1: completed
Output: Hello Anson
```

Two things are worth noticing:

- **The run parks, it doesn't block.** `wait=True` returns as soon as the agent
  asks, because it can't make progress without you. The run sits at status
  `"waiting"` — you can kill the worker here and it picks up where it left off.
  Answering is just another message.
- **The conversation key matters.** It keeps every message pointed at the same
  agent process, so the answer reaches the agent that asked. Without one, each
  `send_message` starts a fresh run.

After you answer, the agent calls `wait` for 10 seconds before greeting you.
That's the durability demo: kill the worker during those 10 seconds, restart it,
and the run resumes from the event log rather than starting over.

## License

MIT
