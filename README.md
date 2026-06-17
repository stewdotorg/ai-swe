# Tools, skills and research relevant to doing software engineering with AI

An informal collection of promising, potentially useful agent skills, tools and information resources as I re-orient my software engineering process in the age of AI agents.

The motivation is to extend my (and my agents') awareness of what people are experimenting with and building for this new era, while keeping a prinicpled commitment to hard-won lessons from the engineers who built the world we now inhabit, many of the lessons, best practices, perspectives and strategies still hold important value, especially when confronting the problem of how to build _quality_, _maintainable_, _evolvable_ software. I am especially attracted to experiments, projects, writings and videos that in one way or another align with this perspective.

## Setup

### How to use this repo - git submodules

This repo references several submodules that serve as read-only research material
and AI context. No development or contribution to the submodules is expected.

**Cloning (first time):**

```bash
git clone --recurse-submodules git@github.com:stew/ai-swe.git
```

If you already cloned without `--recurse-submodules`, run:

```bash
git submodule update --init --recursive
```

**Updating all submodules to latest:**

```bash
# Fetch + checkout the latest commit on each submodule's tracked branch
git submodule update --remote --recursive

# Stage the new SHAs and commit them in this repo
git add .
git commit -m "submodules: bump to latest"
git push
```

Each submodule tracks its default branch (`main` for most, `master` for trafilatura),
configured in `.gitmodules`. The commands above fast-forward every submodule to
the tip of that branch, then record the new pins so other clones get the same
snapshot.

If you ever need to revert submodules to the pinned commits recorded in this
repo (without pulling new changes), run `git submodule update --recursive`
(omit `--remote`).

**Why submodules?**
These external repos evolve independently. Submodules pin them at known-good
revisions so that AI tools and readers get a consistent snapshot. When there's
new material worth surfacing, bump the pins deliberately.

[pocock-talk]: https://www.youtube.com/watch?v=-QFHIoCo-Ko&list=PLgWBLnrVaSruEA5HL8Yw0MNRfVmiuDhgJ
