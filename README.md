# webOS Clean Launcher

Minimalist home screen for LG webOS TVs: greeting and clock, a row of apps, a row of
inputs, and a way back to LG's own home. Built for **webOS 6 (Chrome 79)**.

Based on [partridgeworks/webos-launcher](https://github.com/partridgeworks/webos-launcher)
(MIT). App ID: `com.pplferreira.webos-clean-launcher`.

- **Apps**: your choice of apps, in your order
- **Sources**: Live TV and HDMI inputs (names from the TV's input settings)
- **More**: LG's home screen and Settings
- Black background, remote-first navigation, follows the TV's 12/24-hour clock
- No root, no background service, no startup scripts

## Using it

| Where | Key | Does |
|---|---|---|
| Home | ↑ ↓ | Move between rows |
| Home | ← → | Move along a row |
| Home | OK | Open the app or input |
| Settings | OK | Add or remove an app/source |
| Settings | Hold OK (or **Re-order** button) | Enter/leave re-order mode |
| Re-order mode | OK, arrows, OK | Pick up a tile, move it, put it down |
| Re-order mode | Back | Cancel the move / leave re-order mode |
| Settings | Back | Return home |

The launcher does not take over the HOME button. On a rooted TV you can map a button to it
with [LG Input Hook](https://github.com/Simon34545/lginputhook) (app ID
`com.pplferreira.webos-clean-launcher`).

## Install

Precisa do pacote `.ipk`:
`com.pplferreira.webos-clean-launcher_<versão>_all.ipk`
(gerado com `npm run package`, ou o artifact `webos-clean-launcher.bundle`,
que é o mesmo arquivo).

Funciona no **LG Developer Mode**, sem root. PC e TV precisam estar na **mesma rede**.

Guia completo: [docs/instalacao-developer-mode.md](docs/instalacao-developer-mode.md).

### Resumo (Developer Mode + Dev Manager)

1. Conta em [webostv.developer.lge.com](https://webostv.developer.lge.com).
2. Na TV: LG Content Store → **Developer Mode** → instalar → login → **Dev Mode Status** ON (reinicia) → **Key Server** ON.
3. No PC: [webOS Dev Manager](https://github.com/webosbrew/dev-manager-desktop/releases) → Add Device (IP da TV, porta **9922**, user **`prisoner`**, passphrase da TV) → Install → escolha o `.ipk` / `.bundle`.
4. Abra **Clean Launcher** na lista de apps da TV.

Alternativas: Homebrew Channel (TV com root) ou
[webOS CLI](https://webostv.developer.lge.com/develop/tools/cli-installation) (`ares-install`).

## Build

Needs Node 18+.

```sh
npm install
npm run build      # type-check, then build dist/ (chrome79, classic script)
npm run package    # dist/ → build/com.pplferreira.webos-clean-launcher_<version>_all.ipk
```

To install on a rooted TV over SSH, copy `tv.env.example` to `tv.env` and set `TV_HOST`,
then:

```sh
npm run install:tv   # copy the ipk over SSH, install it, launch it
npm run deploy       # build + package + install:tv
```

`npm run dev` serves the UI in a desktop browser with sample data.

## License

MIT — see [LICENSE](LICENSE) and [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).
