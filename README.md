# @lilsnibbi/toolkit

Small helper functions for the [Bun](https://bun.sh/) runtime. No dependencies.

The package ships raw TypeScript — there is no build step and no compiled
output. Consumers need Bun, or a bundler that resolves `.ts` imports.

```bash
bun add @lilsnibbi/toolkit
```

The logger and the discord.js structures that used to live here are now their
own packages: [`@lilsnibbi/logger`](https://www.npmjs.com/package/@lilsnibbi/logger)
and [`@lilsnibbi/discord-kit`](https://www.npmjs.com/package/@lilsnibbi/discord-kit).

## Helpers

```ts
import {
  chunk,
  clamp,
  formatBytes,
  formatSeconds,
  isLink,
  randomInt,
  randomItem,
  shuffle,
  toOrdinal,
  truncate,
} from "@lilsnibbi/toolkit";

chunk([1, 2, 3, 4, 5], 2);                 // [[1, 2], [3, 4], [5]]
clamp(15, 0, 10);                          // 10
formatBytes(1500);                         // "1.5 KB"
formatBytes(1536, { binary: true });       // "1.5 KiB"
formatSeconds(9000);                       // "2 hours and 30 minutes"
formatSeconds(9000, { format: "short" });  // "2h 30m"
isLink("https://example.com");             // true
randomInt(1, 6);                           // a d6 roll
randomItem(["rock", "paper", "scissors"]); // one of the three
shuffle([1, 2, 3, 4, 5]);                  // a new array, random order
toOrdinal(22);                             // "22nd"
truncate("Hello, world!", 8);              // "Hello..."
```

| Export | Summary |
| --- | --- |
| `chunk(array, size)` | Splits an array into chunks of at most `size`. Throws `RangeError` unless `size` is a positive integer. |
| `clamp(value, min, max)` | Clamps into the inclusive range. Throws `RangeError` if `min > max`. |
| `formatBytes(bytes, options?)` | Human-readable size; decimal units by default, `binary: true` for KiB/MiB. See `FormatBytesOptions`. |
| `formatSeconds(seconds, options?)` | Calendar-aware duration formatter. See `FormatSecondsOptions`. |
| `isLink(value)` | Whether a string parses as an absolute URL (any scheme). |
| `randomInt(min, max)` | Uniform integer in `[min, max]`. Not cryptographically secure. |
| `randomItem(array)` | Uniform random element. Throws `RangeError` on an empty array. |
| `shuffle(array)` | Fisher–Yates over a copy; the input is not modified. |
| `toOrdinal(value)` | English ordinal via `Intl.PluralRules`. |
| `truncate(value, maxLength)` | Shortens to `maxLength`, ellipsis included in the budget. |

`formatSeconds` is calendar-aware: years and months are measured against the
current date rather than fixed averages, so leap years and varying month
lengths are respected. It also takes `onlyUnits`, `includeZeroUnits`,
`rounding` and a `customFormatter`. Exported types: `FormatSecondsOptions`,
`TimeUnit`, `RoundingMode`, `DurationFormat`, `FormatBytesOptions`.

## Development

```bash
bun test                          # run the suite
bun run check                     # typecheck, tests, release metadata, biome
bun run pretty                    # format
```

Releases use Conventional Commits and Release Please. Merge the generated release PR
after CI passes to create the version, changelog, GitHub release, and npm package.
Configure `RELEASE_TOKEN` and `NPM_TOKEN`; see
[.github/RELEASE_POLICY.md](.github/RELEASE_POLICY.md).

## License

MIT
