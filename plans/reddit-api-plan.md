# Reddit API Integration Plan
**Project:** 40k Weekly Digest  
**Status:** Deferred — ready to implement when triggered  
**Last updated:** 2026-05-22

---

## Why this plan exists

The 40k Weekly Digest currently sources Reddit data via general web search. This works, but it has limitations:

- Web search surfaces articles *about* Reddit posts, not the posts themselves
- Upvote counts, comment counts, and post velocity are estimates, not real figures
- Hot and trending posts can be missed entirely if they haven't been indexed

The Reddit JSON API solves all of this. It gives Claude direct access to structured post data — real upvote counts, exact comment numbers, precise timestamps, and the ability to query multiple subreddits in a single pass. The result is a significantly more accurate and reliable digest.

---

## What changes when this is implemented

The scheduled task will continue to run every Monday at 8:00 AM. The only difference is that for Reddit specifically, instead of using web search, Claude will fetch data directly from Reddit's API. Everything else — Google Sheets output, HTML generation, GitHub push — stays exactly the same.

---

## Part 1 — Your setup steps (non-technical)

These steps take approximately 10–15 minutes total. You only do this once.

### Step 1 — Log in to Reddit
Go to [reddit.com](https://www.reddit.com) and log in to your existing account.

### Step 2 — Open the app registration page
While logged in, go to: **https://www.reddit.com/prefs/apps**

Scroll to the bottom and click **"are you a developer? create an app..."**

### Step 3 — Fill in the app registration form

| Field | What to enter |
|-------|---------------|
| Name | `40k-digest` |
| App type | Select **script** |
| Description | Leave blank |
| About URL | Leave blank |
| Redirect URI | `http://localhost:8080` |

Click **"create app"** when done.

### Step 4 — Copy your credentials

- **Client ID** — short string directly under the app name (below "personal use script")
- **Client Secret** — labeled "secret" on the card

Copy both somewhere safe.

### Step 5 — Tell Claude you're ready

> "I'm ready to implement the Reddit API. Here are my credentials: Client ID: [paste], Client Secret: [paste], Username: [paste]"

Claude will handle everything from that point.

---

## Part 2 — Implementation notes for Claude

### Authentication
Use the script app / password flow (OAuth2):
```
POST https://www.reddit.com/api/v1/access_token
Authorization: Basic base64(client_id:client_secret)
Body: grant_type=password&username={username}&password={password}
User-Agent: 40k-digest/1.0 by u/{username}
```

### Subreddits to query
r/Warhammer40k, r/killteam, r/AdeptusMechanicus, r/spacemarines, r/WarhammerFantasy, r/Warhammer, r/ageofsigmar, r/horus_heresy

### Key endpoints
```
GET https://oauth.reddit.com/r/{subreddit}/top?t=week&limit=25
GET https://oauth.reddit.com/r/{subreddit}/hot?limit=25
Authorization: Bearer {token}
User-Agent: 40k-digest/1.0 by u/{username}
```

### Data fields per post
title, score, upvote_ratio, num_comments, created_utc, subreddit, author, permalink, link_flair_text, num_crossposts

### Velocity calculation
```
velocity_score = score / max(hours_since_posted, 1)
```

### Future enhancements post-Reddit-API
- Email delivery via Zapier
- Mobile push notifications via Zapier
- Reddit comment sentiment analysis
