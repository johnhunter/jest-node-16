# Jest with node 16

This repo was created to expose the memory issue with jest in the latest
version of node.

When using jest with node 16.11+, the memory usage can easily go out of
control even for very simple test suites like those used in this repo.

_Note: This fork uses Volta for running tests with differnent node versions. See [relevant documentation here](https://docs.volta.sh/reference/run)._

## Results

Test Suites: 100 total  
Tests: 100000 total

| Node  | Jest   | config    | heap MB (Δ)     | time secs |
| ----- | ------ | --------- | --------------- | --------: |
| 14.20 | 28.1.3 |           | 40 -> 646 (606) |      37.9 |
| 14.20 | 29.0.3 |           | 46 -> 418 (372) |     40.26 |
| 14.20 | 29.0.3 | expose-gc | 25 -> 86 (61)   |      50.7 |
| 16.10 | 29.0.3 |           | 54 -> 379 (325) |     40.81 |
| 16.16 | 28.1.3 |           | 40 -> 896 (856) |     47.46 |
| 16.16 | 29.0.3 |           | 46 -> 819 (773) |     50.72 |
| 16.16 | 29.0.3 | expose-gc | 28 -> 356 (328) |     72.86 |

### Notes:

- Jest 29 seems to have reduced memory leaks although increasing run time.
- Node 16 has increased the leaked memory since 16.10.
- `expose-gc` allows Jest to trigger the node CG after each test. Node optimises performance by using a [large heap old-space](https://www.nearform.com/blog/tracking-memory-allocation-node-js/) - sacrificing memory use to get speed. Therefore we would expect using the `expose-gc` flag would result in a smaller heap delta but an increased run time.
