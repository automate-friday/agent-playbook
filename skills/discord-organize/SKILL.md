---
name: discord-organize
description: Organize and clean up Discord server sidebars by auditing, restructuring, and managing channels and categories. Use when asked to organize discord, clean up sidebar, reorganize channels, reorder categories, archive unused channels, move channels between categories, rename channels, or restructure a Discord server layout.
---

# Discord Organize

Reorganize Discord server channels and categories using the `message` tool's channel management actions. For channel type IDs and parameter details, read [references/channel-types.md](references/channel-types.md).

## Workflow

### 1. Audit Current State

```
message action=channel-list
```

Parse the result into a tree: categories → child channels (grouped by `parentId`). Identify:
- Uncategorized (top-level) channels
- Empty categories
- Channels with unclear or inconsistent names
- Inactive/unused channels (low activity, stale topics)

Present the current structure as a readable tree to the user.

### 2. Propose Reorganization

Present a before/after comparison showing:
- Which channels move where
- New categories to create
- Categories to delete (must be empty first)
- Channels to rename
- Channels to archive or delete

**Wait for user approval before executing any changes.**

### 3. Execute Changes

Apply changes in this order to avoid conflicts:

1. **Create new categories** — `message action=category-create name="..." position=N`
2. **Move channels** — `message action=channel-move channelId="..." parentId="..." position=N`
3. **Rename channels** — `message action=channel-edit channelId="..." name="..."`
4. **Reorder channels** — `message action=channel-edit channelId="..." position=N`
5. **Delete empty categories** — `message action=category-delete channelId="..." reason="..."`
6. **Archive channels** — `message action=channel-edit channelId="..." name="archived-..."` then move to an archive category

Report each change as it completes. Summarize final state when done.

## Guidelines

- **Never delete channels with content** without explicit user confirmation
- **Archive over delete** — Rename and move to an "Archive" category instead of deleting
- **Batch related moves** — Group channel moves by target category
- **Preserve permissions** — Moving channels may inherit new category permissions; warn the user
- Position values are zero-indexed; lower = higher in sidebar
