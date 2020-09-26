This repo tries to reproduce an issue I'm running into with caching docker layers on github actions, based on the steps at https://github.com/marketplace/actions/build-and-push-docker-images#leverage-github-cache.

How I expect to work (and how it works for me with regular docker build):

- run docker build with `deploy/Dockerfile`
- change code in file `main.rs`
- run the same docker build again
- everything until `COPY . /src/testcase2/` should be cached

By mostly reproducing the commands that github actions run with docker buildx, the import/export of cache doesn't seem to work in the same way

- run docker buildx with `deploy/Dockerfile`, export the cache to some local directory using `--cache-to`
- change code in `main.rs`
- run `docker buildx prune --all`, to remove cache on the machine (shouldn't affect the exported cache from previous steps) - this simulates the fact that a github actions build starts from a fresh machine (where there wouldn't be any cache yet)
- run the same docker buildx again, using --cache-from pointing at the local directory where cache was previously exported to
- all layers in the first stage are rebuilt from scratch