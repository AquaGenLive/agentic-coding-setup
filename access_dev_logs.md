# Access dev logs
so the coding agent can check exceptions and behaviour of the app on it's own.

## Setup for NPM scripts

In the package.json edit the dev script so that the app run command ends with:
```
2>&1 | tee logs/dev.log
```

E.g. when running a backend (Java) and a React.JS frontend it could look like this:
```
"dev": "concurrently -n backend,frontend -c blue,green \"npm run dev:backend\" \"npm run dev -w frontend\" 2>&1 | tee logs/dev.log"
```

This will create a dev.log file inside a logs folder. As tee is not adding this logs folder on it's own you need to manually create it.

### Tell the AI agent where to find the logs
Edit your ```AGENTS.md``` and/or ```CLAUDE.md``` with that text:

```
## Dev server logs
Dev server logs are in `logs/dev.log`. Check this file when debugging runtime errors.
```