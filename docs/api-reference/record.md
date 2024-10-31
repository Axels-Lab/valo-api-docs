# Record Related API Endpoints

## [Base URL](./overview.md/#base-url)

```plaintext
https://valo-api.vercel.app
```

## `/record` Endpoint

### Description

The `/record` endpoint provides information about the user's Valorant MMR history during the current stream. It calculates wins, losses, draws, and RR (Rank Rating) changes based on the stream's start time.

### HTTP Method

`GET`

### Endpoint

`/record`

### Request Parameters

| Parameter | Type   | Description                                                                      | Example                                     |
| --------- | ------ | -------------------------------------------------------------------------------- | ------------------------------------------- |
| `channel` | String | The Twitch channel name of the streamer.                                         | `Streamer123`                               |
| `ign`     | String | The in-game name of the Valorant player.                                         | `PlayerName`                                |
| `tag`     | String | The tagline associated with the player's in-game name, excluding the `#` symbol. | `1234`                                      |
| `region`  | String | The region of the player's Valorant account (e.g., `na`, `eu`).                  | `na`                                        |
| `apiKey`  | String | Your API key obtained from HenrikDev API                                         | `HDEV-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` |

### Example Request

```plaintext
GET /record?channel=Streamer123&ign=PlayerName&tag=1234&region=na&apiKey=HDEV-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### Example Response

```plaintext
Wins: 3, Losses: 2, Draws: 1 | RR change: +45 RR this stream
```

### Command Setup

Below are examples of how to use the `/record` endpoint in various chatbot commands. Replace the placeholders with actual values.

To add a new command in your chatbot, use the following syntax:

=== "Nightbot"

    ```plaintext
    !addcom !record $(urlfetch https://valo-api.vercel.app/record?channel=$(channel)&ign=<username>&tag=<tag>&region=<region>&apiKey=<your-api-key>)
    ```

=== "StreamElements"

    ```plaintext
    !cmd add !record $(customapi https://valo-api.vercel.app/record?channel=${channel}&ign=<username>&tag=<tag>&region=<region>&apiKey=<your-api-key>)
    ```

=== "Fossabot"

    ```plaintext
    !addcmd !record $(customapi https://valo-api.vercel.app/record?channel=$(channel)&ign=<username>&tag=<tag>&region=<region>&apiKey=<your-api-key>)
    ```

To edit an existing command, use the following syntax:

=== "Nightbot"

    ```plaintext
    !editcom !record $(urlfetch https://valo-api.vercel.app/record?channel=$(channel)&ign=<username>&tag=<tag>&region=<region>&apiKey=<your-api-key>)
    ```

=== "StreamElements"

    ```plaintext
    !cmd edit !record $(customapi https://valo-api.vercel.app/record?channel=${channel}&ign=<username>&tag=<tag>&region=<region>&apiKey=<your-api-key>)
    ```

=== "Fossabot"

    ```plaintext
    !editcmd !record $(customapi https://valo-api.vercel.app/record?channel=$(channel)&ign=<username>&tag=<tag>&region=<region>&apiKey=<your-api-key>)
    ```

### Command Usage

- `!record` - Retrieves the user's Valorant MMR history during the current stream.
- Replace `<username>`, `<tag>`, `<region>`, and `<your-api-key>` with the appropriate values.
- `$(channel)/${channel}` is a bot variable that automatically fetches the channel name. Which is in turn used to calculate uptime, stream start time, etc. for showing accurate MMR history.

### Additional Notes

- The API uses the HenrikDev API internally to fetch the player's match details.
- Ensure that you have a valid API key to access the endpoints.
- The API responses are in plain text format suitable for chatbot commands.
- For detailed error handling and response formats, refer to the specific endpoint documentation.

### External API Used

- [https://api.henrikdev.xyz/valorant/v2/mmr-history](https://docs.henrikdev.xyz/valorant/api-reference/mmr-history#valorant-v2-mmr-history-region-platform-name-tag){target="\_blank"} - HenrikDev API for fetching mmr-history details.

### Limitations

- The `!record` command assumes that every game with negative RR is a loss and any game with >7 RR is a win. Anything in between is considered a draw. While this works 98% of the time, this logic is still flawed and shall be improved in the future.
- Additionally, it can only see 20 games worth of data at maximum.
