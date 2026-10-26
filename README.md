# Sumidoku OAuth relay

Static GitHub Pages site that relays tracker OAuth callbacks to the Sumidoku
instance that started the sign-in.

Tracker providers (AniList, MyAnimeList, Bangumi, MangaBaka) only accept
pre-registered redirect URIs. A self-hosted Sumidoku instance runs on an
arbitrary host, so all instances share this fixed endpoint:

- the provider returns to `https://sumidoku.github.io/oauth/<provider>/callback/`
- the relay page reads the instance's own callback URL out of the OAuth
  `state` parameter and redirects the browser there, forwarding `code`,
  `state` and any error parameters untouched
- the instance then exchanges the code for tokens, sending this relay URL as
  the `redirect_uri`

The instance-side validity check (the `state` nonce against its pending
table) happens in Sumidoku itself; the relay keeps no state and logs nothing.

## Layout

- `index.html` — what this site is
- `<provider>/callback/index.html` — the relay (identical for all four
  providers)
- `404.html` — everything else
