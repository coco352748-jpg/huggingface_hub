---
name: sharing-images-and-videos
description: Upload an image or video to GitHub's user attachments API and embed the hosted link directly in your own message response to the user. Use whenever you want to share a screenshot, diagram, recording, or other media in your reply — not for attaching media to a pull request or issue.
user-invocable: false
---

# Sharing Images and Videos

Use this skill whenever you want to include an image or video directly in your own
message response to the user — for example, showing a screenshot you captured, a
diagram you generated, or a recording you produced.

This skill is **not** for attaching media to a pull request or issue description or
comment — use the dedicated skill for that instead.

## When to use it

- Showing a screenshot you took (e.g. of a running app, a browser, or a UI change)
- Sharing a diagram that clarifies architecture or flow
- Including a short recording that makes behavior easier to understand
- Turning a local media file into a GitHub-hosted URL that can be linked from markdown
  in your response

Only use this when visuals genuinely improve understanding.

## Instructions

1. Put the real values in shell variables first, so the untrusted filename is never
   pasted into the middle of another command. Set `REPO` to the current repository's
   `owner/repo` — do **not** assume it if you're unsure, look it up instead:

    ```bash
    FILE='assets/dashboard.png'          # path to the media file on disk
    NAME="$(basename -- "$FILE")"         # display name shown in the attachment
    MIME='image/png'                      # actual MIME type (e.g. video/mp4 for video)
    REPO='OWNER/REPO'                     # owner/repo for the current repository
    ```

    Never build these from `$(...)` command substitution embedded in an untrusted
    filename — assign the filename to `FILE` with single quotes, then reference `"$FILE"`.

2. Resolve the repository database id for `REPO`, failing loudly if the lookup does not
   return a numeric id:

    ```bash
    REPO_ID="$(gh api "repos/$REPO" --jq .id)" || { echo "repo lookup failed" >&2; exit 1; }
    case "$REPO_ID" in ''|*[!0-9]*) echo "no repository_id for $REPO" >&2; exit 1;; esac
    ```

3. Upload the raw media bytes to GitHub. Everything untrusted stays inside a quoted
   variable, and `--url-query` URL-encodes each value:

    ```bash
    URL="$(curl --fail-with-body -sS -X POST \
      "https://uploads.github.com/user-attachments/assets" \
      --url-query "name=$NAME" \
      --url-query "content_type=$MIME" \
      --url-query "repository_id=$REPO_ID" \
      -H "Content-Type: application/octet-stream" \
      -H "X-GitHub-Api-Version: 2022-11-28" \
      -H "Authorization: Bearer $(gh auth token)" \
      --data-binary "@$FILE" | jq -r .url)"
    case "$URL" in https://*) ;; *) echo "upload failed: $URL" >&2; exit 1;; esac
    ```

4. `URL` now holds the hosted attachment link, which looks like:

    ```text
    https://github.com/user-attachments/assets/...
    ```

5. Embed the hosted URL in markdown directly in your response to the user:

    ```text
    ![alt text](url)
    ```

    Use `![alt text](url)` for images. For video or other non-image media, paste the URL
    on its own line in your response (GitHub renders a player) or use a plain markdown
    link if that reads better in context.

## Important details

- Send the file as raw binary bytes with `--data-binary "@$FILE"`. Do **not** use
  multipart form uploads, base64 encoding, or JSON wrappers.
- Put `name`, `content_type`, and `repository_id` in the query string via `--url-query`,
  which URL-encodes each value. Never interpolate them directly into the URL — a filename
  with spaces, `&`, `#`, or other reserved characters would corrupt the request.
- Keep untrusted filenames inside quoted shell variables; never paste them into the body
  of another command where shell metacharacters could be evaluated.
- `--url-query` needs curl >= 7.87 and `--fail-with-body` needs curl >= 7.76. On older
  curl, encode the query manually and use `--fail --show-error` (which exits nonzero but
  discards the error body).
- Always confirm a `https://` `url` came back before embedding it, so an expired token or
  4xx/5xx response fails the task instead of silently succeeding.
- For videos, keep the same request shape and set `MIME` to the real video MIME type, for
  example `video/mp4`.

## When not to use it

- Text-only responses where the diff or explanation already covers everything
- Cases where a simple markdown list or code snippet is clearer than an image
- Attaching media to a pull request or issue description or comment — use the
  dedicated skill for that instead
