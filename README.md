# token-radar-01

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/gc-cheng/token-radar-01/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/gc-cheng/token-radar-01/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Token Launch Radar: a web dashboard that tracks pre-token DeFi protocols and ranks them by the implied value of a launch, with a farm plan for each one. It reads a sealed DefiLlama snapshot, scores every protocol with one transparent model, and renders the board, a detail view for every protocol, and the steps to farm it.

The board covers the whole watchlist — every protocol with at least $50M of TVL and no token on record — plus every rankable pre-token protocol below that floor, so it finds launches before they are big. Implied value follows only from the snapshot's own figures: TVL, seven-day growth, age with no token, and the number of chains. No protocol name is special-cased and no row is hand-picked. Live-token protocols are set aside with their market caps shown, never mixed into the ranking.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input token-radar-01-kit/input/snapshot.json
```

`main.ts` (Node 22, standard library only) reads `/input/snapshot.json` in the grader, writes the ranked board to `/output/board.json`, writes the dashboard's data to `/output/data.js`, copies `dashboard/` and `screenshots/` to `/output`, and writes `/output/dossier.json` with the vendored make.fun library. No network, no timestamps, no randomness — two runs write identical bytes.

## The model (radar-2)

```
implied = round( tvl_usd x 0.22 x (1 + clamp(change_7d, -60, 120) / 100 x 0.6)
               x (0.55 + min(age_days, 1095) / 1095 x 0.45)
               x (0.95 + 0.02 x min(chains, 5)) )
```

TVL is the value users already trust the protocol with; seven-day growth is the only momentum the snapshot carries; age with no token is the best record of how long a launch has been waited for; chain breadth is the protocol's distribution. Each farm plan is derived from the protocol's own record — category, chains, TVL and age — with six steps: a fresh wallet, the category's action, weekly qualifying activity, a hold of 30–60 days, claiming only from the protocol's own published link, and a farm log.

## License

MIT — see `LICENSE`.

---

<sub>Entered on [make.fun](https://make.fun/?ref=3pf76s6q&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
