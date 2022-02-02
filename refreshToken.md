We have an issue with our SSO sessions in the white label apps. It has specifically been identified by Coegco and GCI, who both handle SSO refreshes the same way:

An example from a Partner... On the initial auth, we receive:
```
{
    "access_token": "eaoX6p90OeA-qI6V25tPKdzF49w",
    "refresh_token": "WpzNbHvBapcf6HNOgqb35ApSJ3k",
    "scope": "plume",
    "token_type": "Bearer",
    "expires_in": 86399
}
```
and on a refresh, we get:
```{
    "access_token": "pvA2bm-SBiISZDCoLAX0f8y8DCk",
    "scope": "plume",
    "token_type": "Bearer",
    "expires_in": 86399
}
```
- Note that there's no `refresh_token` in this response.

- Unfortunately, the apps do not retain the `refresh_token` so we get a new session that can't be refreshed. It expires after the TTL and the user must create a new session.
- The app clears our webview cache each time it closes (to solve an unrelated login bug) so we don't retain the user's browser cookie, requiring a full auth flow. 
- There's also an (unrelated) issue preventing native password managers from working properly on Android.

So all in all, this is a very painful experience for the Partner and their customers

I see two options to resolve this:
1. We update the apps to persist the refresh_roken until we're provided a replacement
2. We update the Piranha provisioning microservice to merge the supplied refresh_token with the IDP's response body before forwarding it to the apps

I see two issues with the first option:
- We aren't planning any more development on the legacy apps, full stop. So we'd have a pretty high barrier of entry to get this work approved, groomed and planned before Monday when the next sprint starts. We'd also be under the gun to implement the solution for all of our partners and deploy in a 80 or 82 patch version
- We don't have first hand knowledge of the IDP's response since it goes through the Cloud first. I could see some scenarios where we're expecting a partner to not provide a new refresh_token so we keep our existing one, but then it is invalidated and we don't have a reliable way to clear it
