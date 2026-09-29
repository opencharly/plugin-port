# plugin-port

Listening-port probing for OpenCharly — the `port:` check verb.

The verb probes a TCP port on a live deployment: an in-container listening check
(`ss`/`netstat`) under `charly check box`, or a host-side TCP reachability dial.
It is a host-coupled check verb whose `RunVerb` runs against the live check
engine (`sdk/kit.CheckContext`), served in **either** placement — compiled into
`charly`, or out-of-process over the `CheckContextService` reverse channel.

## What it provides

| Capability | Surface |
|---|---|
| `verb:port` | the `port:` check verb — assert a port is listening (`port:`, optional `listening:` defaulting to `true`) |

## How to use it

Compose the plugin candy in a box or check bed's `candy:` list:

```yaml
- '@github.com/opencharly/plugin-port/candy/plugin-port:<tag>'
```

Then author the verb in a plan:

```yaml
- check: the ssh port is listening
  id: port-ssh
  port: {port: 22, listening: true}
  context: [runtime]
```

## Layout

- `candy/plugin-port/` — the plugin module: `plugin.go` (the `verb` +
  `NewCheckVerb()`/`NewMeta()`), `schema/port.cue` (the self-contained
  `#PortInput`), `params/cue_types_gen.go`, `cmd/serve/main.go`.
- `charly.yml` — the root project manifest (`discover: candy`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.

## Related

- Owning skill: `/charly-check:check` — the check verb catalog. This candy
  carries no `skill:` entity of its own; the gap is tracked in
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).
- `/charly-internals:plugin` — the plugin/provider model.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI.
