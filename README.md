# hello-bot

A hello-world agent on [Norns](https://github.com/nornscode/norns). Good starting point for seeing how workers and clients work with the [Python SDK](https://github.com/nornscode/norns-sdk-python).

## Run

Start a local Norns server:

```sh
brew install nornscode/tap/nornsctl
nornsctl dev
```

Start the worker:

```sh
uv sync
uv run hello-worker
```

In another terminal, send a message:

```sh
uv run hello-client
```

## License

MIT
