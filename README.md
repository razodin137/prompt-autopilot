# prompt-autopilot

Point an agent at this file and give it a repo. It writes one pack; the runner
replays the pack one prompt per fresh session, stopping at the first failing gate.

## The pack

The runner reads `prompt/`. One feature per file, numbered in the order they run:

    00-overview.md
    01-<feature>.md
    02-<feature>.md

If two things could ship separately, they are two files. More files beats more
prose. `prompt-once/` and `prompt-listmonk/` are the shape to copy;
`prompt-template/` is a worked pack: a small reading list, one feature per file,
showing the order and the escalation. Copy it, retarget the domain, keep the
shape.

A pack runs in the repo it changes. Copy the chosen pack into that repo's
`prompt/` and the runner into its `scripts/`; this repo is the workshop.

## The prompt

Every file except the overview is 2-6 lines plus this exact ending:

    Your job is to go in and add <feature> here to this <stack> app.

    <The one thing it already has that this builds on.>

    Done is when <gate> runs clean and so do house-style lints

    /home/mrjohn/Work/tries/2026-11-03-house-style/house-style

The first line names the stack. "Here" is the repo the agent is already in. The
done line and the house-style path are identical in every file of a pack, byte
for byte.

Rules:

- One feature. A prompt that needs "and" is two prompts.
- Say what already exists when it matters: "there is page reordering already;
  add book reordering."
- Say what it should feel like when it matters. A wii/ps3 menu, rotating game
  disks, a cream background is a complete design instruction. Do not translate
  it into hex codes and easing curves.
- No headings, bullets, frontmatter, file paths, class names, method signatures,
  gotchas, or acceptance criteria. The agent opens the repo and finds those.
- Too big to describe? Split it. Never add anchors, cut lists, or a "why" essay.

One feature file, in full:

    Your job is to go in and add signup cards here to this Go app.

    A card is a named signup form that pins one or more labels. The lists stay
    lists underneath; the card is the thing you hand out.

    Done is when make test runs clean and so do house-style lints

    /home/mrjohn/Work/tries/2026-11-03-house-style/house-style

## The overview

`00-overview.md` is 3-4 lines: what is being changed, that it is one feature per
file and each runs in a fresh session, the house-style path, and what the last
line of every file means.

## The gate

In the target repo, before writing a line:

    CHECK_ONLY=1 ./scripts/split-serial.sh

That prints the pack, the count, and the gate. The gate is `CHECK` if you set
it, otherwise `npm run check` when `package.json` declares one. No gate means no
gate — say so plainly rather than guessing, and pass `CHECK` explicitly:

    CHECK='make test' ./scripts/split-serial.sh

Copy the printed gate into every done line, word for word.

## The runner

One copy, in `scripts/`:

    ./scripts/split-serial.sh
    ./scripts/split-serial.sh ../packs/listmonk
    FROM=05-shell COMMIT=0 ./scripts/split-serial.sh

Fresh session per file, one commit per green step, logs in `.scratch/split/`.
The tree must be clean when committing is on; `COMMIT=0` skips both the check
and the commits.

## Before you hand it over

- Every feature is one file, and every file is one feature.
- Every done line matches `CHECK_ONLY=1` exactly.
- No file has a heading, bullet, frontmatter, or file path.
- Numbering runs 00, 01, 02 and so on, with no gaps.
