# Spotify Recap

Spotify Recap is a planned service that tracks curated playlists and emails subscribers about new additions.
The project will manage playlists and their subscriber lists in a JSON file and send periodic update emails
with details like who added a song and relevant genre information.

## Architecture Overview

```
+-------------------+
| playlists.json    |  <-- configuration for playlists, subscribers,
|                   |      and update frequency
+-------------------+
          |
          v
+-------------------+     +------------------+
| Playlist Tracker  | --> | Email Generator  |
| (polls Spotify    |     | (HTML templates) |
|  for changes)     |     +------------------+
+-------------------+             |
          |                       v
          |               +------------------+
          +-------------->| Email Delivery   |
                          | (e.g., SMTP or   |
                          |  API service)    |
                          +------------------+
```

- **Configuration**: `playlists.json` stores playlist IDs, subscriber emails, and a `frequency`
  field (`weekly` or `monthly`).
- **Playlist Tracker**: Queries the Spotify API to detect additions to each playlist since the last run.
- **Email Generator**: Builds aesthetically pleasing recap emails using HTML/CSS templates.
- **Email Delivery**: Sends emails via an SMTP server or third-party provider like SendGrid.
- **Scheduler**: A cron job or serverless scheduler triggers the process at the appropriate frequency.

## Data Model

Example `playlists.json`:

```json
{
  "chill_hits": {
    "playlistId": "spotify:playlist:1",
    "frequency": "weekly",
    "subscribers": ["alice@example.com", "bob@example.com"]
  },
  "workout_mix": {
    "playlistId": "spotify:playlist:2",
    "frequency": "monthly",
    "subscribers": ["carol@example.com"]
  }
}
```

## Workflow

1. Scheduler triggers the job (weekly or monthly).
2. Playlist Tracker reads `playlists.json` and checks Spotify for new tracks.
3. For each new track, collect metadata (added by, track details, genre changes).
4. Email Generator composes an HTML email summarizing updates and recommendations.
5. Email Delivery sends the recap to each playlist's subscribers.

## Development Roadmap

1. **Project Setup**
   - Initialize Node.js project and install dependencies (Spotify Web API client, email SDK, task scheduler).
   - Define `playlists.json` schema and validation.

2. **Playlist Tracking**
   - Implement Spotify authentication and polling for playlist changes.
   - Persist last processed track to avoid duplicate notifications.

3. **Email Generation**
   - Create responsive HTML templates for recap emails.
   - Include sections for new additions and catch‑up recommendations.

4. **Scheduler & Delivery**
   - Add cron-based scheduler (e.g., `node-cron`) respecting each playlist's `frequency`.
   - Integrate with an email delivery service.

5. **Testing & Deployment**
   - Unit tests for data parsing and email rendering.
   - Integration tests for Spotify API interactions.
   - Deploy as a containerized service or serverless function.

6. **Future Enhancements**
   - Web dashboard for managing playlists and subscribers.
   - Personalized recommendations based on listener history.

## Contributing

This repository currently contains planning documentation. Future contributions
will include implementation code following the roadmap above.
