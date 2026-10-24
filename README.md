# dsh-plugin-market

Custom [DSH](https://github.com/deepseek-ai/deepseek-harness) plugin market index for two independent plugins:

| Plugin | What it does |
|---|---|
| [dsh-task-plan](https://github.com/nwasvsad/dsh-task-plan) | Off-peak task plan table; pending tasks run as concurrent Sessions. |
| [dsh-task-digest](https://github.com/nwasvsad/dsh-task-digest) | Nightly per-task token/cost/result digest for the previous day. |

## Add the index in DSH

Open **Plugins → Discover → Index sources → Add index source**, then paste:

```text
https://raw.githubusercontent.com/nwasvsad/dsh-plugin-market/main/index.json
```

The two entries can be installed independently. Each install is pinned to the Git commit recorded in [`index.json`](./index.json); the source repositories are:

- https://github.com/nwasvsad/dsh-task-plan (version 0.2.0)
- https://github.com/nwasvsad/dsh-task-digest (version 0.1.0)

## Install without Discover

```powershell
dsh plugin --profile web add github:nwasvsad/dsh-task-plan#6055a4ce0d6fc188c70a1506550d8966ee1af54f
dsh plugin --profile web add github:nwasvsad/dsh-task-digest#a402c0012e0575bc968be21ecb04edb0e564adea
```

Both packages declare `dsh.bundle.patch` and `engines.dsh = ">=0.2.1-rc.2 <0.3.0"`. Git installs run `prepare` to rebuild the browser bundle; approve the build if pnpm asks.

## Verify

```powershell
node --input-type=module -e "import {readFileSync} from 'node:fs'; import {parseMarketIndex} from '@deepseek-ai/dsh-plugin-market-index'; parseMarketIndex(JSON.parse(readFileSync('index.json','utf8')), 'https://raw.githubusercontent.com/nwasvsad/dsh-plugin-market/main/index.json'); console.log('valid')"
```

The index is intentionally a community/custom market: DSH has no official public market, so a user adds this URL explicitly.
