# Discord Channel Types

| Type ID | Name | Description |
|---------|------|-------------|
| 0 | Text | Standard text channel |
| 2 | Voice | Voice channel |
| 4 | Category | Category container (groups channels) |
| 5 | Announcement | News/announcement channel (formerly "news") |
| 13 | Stage | Stage voice channel for events |
| 15 | Forum | Forum channel with thread-based posts |

## Category Behavior

- Channels with `parentId` belong to that category
- Channels with `parentId: null` are uncategorized (top-level)
- Categories themselves have `type: 4` and no `parentId`
- `position` field controls ordering within a category (lower = higher)

## Common Operations via `message` Tool

### Read State
- `channel-list` — Returns all channels with id, name, type, parentId, position
- `channel-info` — Details for a single channel (pass `channelId`)

### Modify Channels
- `channel-edit` — Edit name, topic, position, parentId, nsfw, rateLimitPerUser
- `channel-move` — Move channel to a different category or position (set `parentId` and `position`)
- `channel-create` — Create new channel (set name, type, parentId, position)
- `channel-delete` — Delete a channel (requires `channelId`, add `reason`)

### Manage Categories
- `category-create` — Create a new category (set `name`, optional `position`)
- `category-edit` — Edit category name or position
- `category-delete` — Delete an empty category

## Key Parameters

| Parameter | Used In | Purpose |
|-----------|---------|---------|
| `channelId` | edit, move, delete, info | Target channel |
| `parentId` | create, edit, move | Category to place channel in (null = uncategorized) |
| `position` | create, edit, move, category-create/edit | Order within parent |
| `name` | create, edit, category-create/edit | Channel/category name |
| `type` | create | Channel type (see table above) |
| `reason` | delete, category-delete | Audit log reason |
| `topic` | create, edit | Channel topic/description |
