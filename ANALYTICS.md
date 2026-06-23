# RingChaser Analytics

Analytics events forwarded from the game (`index.html`) to the Sleeper app via the
mini-game bridge.

## How it works

The page includes an inlined `logToApp(props)` helper that mirrors the contract in
[`mini_game_bridge.ts`](../sleeperbot/clients/app-mobile/src/v2/fantasy_tab/components/mini_games/mini_game_bridge.ts).

When the game runs inside the Sleeper app's in-app WebView, it posts a message to the
native app:

```js
window.ReactNativeWebView.postMessage(JSON.stringify({
  msgType: 'mini_game_event',
  props: { action: '...' }
}));
```

When the game runs in a normal browser, every `logToApp` call is a safe no-op and never
throws. The app decides which game an event belongs to (it wires each WebView with its own
`source`), so the game does not set that.

## Events

| Action               | Trigger                                                      | Element     |
| -------------------- | ----------------------------------------------------------- | ----------- |
| `roll_team_clicked`  | User taps the landing-page **ROLL TEAM** button to start    | `#startBtn` |
| `play_again_clicked` | User taps **Play Again** on the results screen              | `#again`    |
| `share_team_clicked` | User taps **📸 Share my team** on the results screen        | `#shareBtn` |

### Payload shape

Each event is sent as a flat object with primitive values:

```json
{ "action": "roll_team_clicked" }
```

## Adding a new event

1. Call `logToApp({ action: 'your_event_name' })` at the point of interest in `index.html`.
2. Keep `props` flat and primitive (`string | number | boolean | null | undefined`).
3. Document the event in the table above.
</content>
</invoke>
