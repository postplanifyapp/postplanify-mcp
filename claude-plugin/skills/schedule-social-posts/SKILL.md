---
name: schedule-social-posts
description: Plan, schedule and review social media posts with PostPlanify. Use when the user wants to post or schedule content to Instagram, TikTok, LinkedIn, X, Facebook, YouTube, Threads, Pinterest, Bluesky or Google Business Profile, check their content calendar, see how posts or accounts performed, or read and reply to comments and reviews.
---

# Scheduling social media posts with PostPlanify

Use the PostPlanify connector's tools. Every post goes to one social account, so posting "everywhere" means one post per account.

## Find the account

1. Call `list_workspaces`. Each workspace lists its connected social accounts with `platform`, `accountName` and `customLabel`.
2. Match what the user said ("our Instagram", "the Downtown page") against those fields. If more than one account matches, ask which one.

## Prepare the post

- **Times are UTC.** Convert the user's local time before calling `create_post`. If you don't know their timezone, ask. For "post now", schedule 2 to 5 minutes ahead.
- **Media:** use `upload_media_from_url` for a public link, `upload_media_from_base64` for a file the user shared, or `list_media` with the `workspaceId` to reuse files from their PostPlanify library. Carousel order follows the order of `mediaIds`.
- **Adapt the caption per platform** when posting the same message to several accounts.

| Platform | Media | Caption limit | Required extra fields |
|---|---|---|---|
| Instagram | 1 to 10 images or videos | 2,200 | |
| TikTok | 1 video, or images | 2,200 | |
| YouTube | exactly 1 video | | `youtubeParams.title` (max 100) |
| Pinterest | exactly 1 image or video | 500 | `pinterestParams.boardId` from `list_pinterest_boards` (ask which board) |
| X | up to 4 | | |
| LinkedIn | up to 20 images, or 1 video, or 1 PDF | | |
| Threads | up to 20 | 500 | |
| Bluesky | up to 4 images, or 1 video | 300 | |
| Facebook | up to 10 images, or 1 video | | |
| Google Business | up to 1 image | 1,500 | `googleBusinessParams` for EVENT or OFFER posts |

Stories (`isStory: true`) work on Instagram and Facebook only, with exactly 1 media file.

## Confirm before acting

- **Before `create_post`:** show the account, the exact caption, the media and the time in the user's timezone, and wait for a yes. Posts publish automatically at their scheduled time.
- **When asked to schedule many posts:** create one or two first, show the result, then continue.
- **Before `reply_to_comment`:** show the reply text. Replies are public and immediate.
- **Before `delete_post`, `delete_media` or `cancel_post`:** confirm the specific item. Deleting cannot be undone.

## Handle errors

- A validation error names the field to fix. Fix it or ask the user; don't resend the same post in a loop, since that risks duplicates.
- A message about the plan or permissions won't change on retry. Tell the user and stop.
- "Maximum posts per hour/day for this account" means picking a different time, not retrying.

## Report results

- After scheduling, reply with the account, the scheduled time in the user's timezone and the post status. Don't paste the raw tool response.
- For performance questions, use `get_brand_overview` for the whole workspace, `get_account_analytics` and `get_analytics_trends` for one account, or `list_posts` with `includeMetrics: true` to compare posts. A null metric means no data has been collected yet; say so instead of reporting zero.
- For the content calendar, use `list_posts` with `from` and `to`, and group the results by day.
