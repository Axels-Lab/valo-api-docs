# Rank Related API Endpoints

This section provides detailed information about the `/rank`, `/checkrank` and `leaderboard-rank` endpoints, which are used to retrieve the current rank, ranking points and leaderboard rank (if applicable) of a specified Valorant player.

## `/rank` Endpoint

### Description

The `/rank` endpoint provides the current rank and ranking points of a specified Valorant player. The API fetches the player's rank and tier details based on their username, tag, and region.

### HTTP Method

`GET`

### Endpoint

`/rank`

### Request Parameters

| Parameter | Type   | Description                                       | Example                                     |
| --------- | ------ | ------------------------------------------------- | ------------------------------------------- |
| `name`    | string | The Valorant username of the player               | `PlayerName`                                |
| `tag`     | string | The Valorant tag of the player                    | `1234`                                      |
| `region`  | string | The region of the player (e.g., `na`, `eu`, `ap`) | `na`                                        |
| `apiKey`  | string | Your API key obtained from HenrikDev API          | `HDEV-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` |

### Response

- **200 OK**  
  Returns a plain text summary of the player's current rank and ranking points.

      **Example Success Response:** `Current Rank: Immortal1 - 45 RR.`

- **400 Bad Request**  
  Returns a plain text error message if required parameters (`name`, `tag`, `region`) are missing.

      **Example Error Response:**
      `Error: Missing parameters. Ensure username, tag, and region are provided.`

- **500 Internal Server Error**  
  Returns a plain text error message if an error occurs while fetching data from the external API (e.g., invalid credentials, player not found, etc.).
  **Example Error Response:** `Error Message from API`

### Command Setup

Below are examples of how to use the `/rank` endpoint in a chatbot command. Replace `<username>`, `<tag>`, `<region>`, and `<apiKey>` with the appropriate values.

To add a new command in your chatbot, use the following syntax:

=== "Nightbot"

    ```bash
    !addcom !rank $(urlfetch https://valo-api.vercel.com/rank?name=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>)
    ```

=== "StreamElements"

    ```bash
    !cmd add !rank $(customapi https://valo-api.vercel.com/rank?name=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>)
    ```

=== "Fossabot"

    ```bash
    !addcmd !rank $(customapi https://valo-api.vercel.com/rank?name=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>)
    ```

=== "Streamlabs Chatbot"

    ```bash
    !command add !rank {readapi.https://valo-api.vercel.com/rank?name=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>}
    ```

To edit an existing command, use the following syntax:

=== "Nightbot"

    ```bash
    !editcom !rank $(urlfetch https://valo-api.vercel.com/rank?name=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>)
    ```

=== "StreamElements"

    ```bash
    !cmd edit !rank $(customapi https://valo-api.vercel.com/rank?name=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>)
    ```

=== "Fossabot"

    ```bash
    !editcmd !rank $(customapi https://valo-api.vercel.com/rank?name=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>)
    ```

=== "Streamlabs Chatbot"

    ```bash
    !command edit !rank {readapi.https://valo-api.vercel.com/rank?name=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>}
    ```

### Command Usage

To use the `!rank` command in your chatbot for the configured user, you can type the following command in your chat:

```bash
!rank
```

---

## `/checkrank` Endpoint

### Description

The `/checkrank` endpoint provides the current and peak rank of a specified Valorant player. This API extracts details based on a single query parameter (`q`) containing the player's username, tag, and region.

### HTTP Method

`GET`

### Endpoint

`/checkrank`

### Request Parameters

| Parameter | Type   | Description                                                                                                                                        | Example                                     |
| --------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| `q`       | string | A single string that contains the player's username, tag, and region, separated by spaces. The tag can be with or without `#` symbol at the start. | `PlayerName 1234 na`                        |
| `apiKey`  | string | Your API key obtained from HenrikDev API.                                                                                                          | `HDEV-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` |

#### `q` Format

- The `q` query parameter should contain **three** components:
  - **Username:** The player's in-game username (can contain spaces).
  - **Tag:** The player's tag (usually a 4-5 digit number or text, without `#`).
  - **Region:** The player's region (e.g., `na`, `eu`).

#### Example `q` Parameter

```http
GET /checkrank?q=PlayerName 1234 na&apiKey=HDEV-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

This `q` value will be split as:

- **Username:** PlayerName
- **Tag:** 1234
- **Region:** na

### Response

- **200 OK**  
  Returns a plain text summary of the player's current and peak rank.

  **Example Success Response:**
  Current Rank: Platinum2 - 0 RR. Peak Rank: Platinum3 in e8a3 (Peak might be incorrect)

- **400 Bad Request**  
  Returns a plain text error message if the required parameters (`username`, `tag`, and `region`) are missing or invalid.

      **Example Error Response:**
      Error: Missing or invalid parameters. Ensure username, tag, and region are provided.

- **500 Internal Server Error**  
  Returns a plain text error message if there is an issue with fetching the data from the external API or any other error occurs.

      **Example Error Response:**
      Error occurred: <Error Message>

### Example Request

```http
GET /checkrank?q=Player%20123 456 na&apiKey=your-api-key
**Example Successful Response**: Current Rank: Immortal1 - 45 RR. Peak Rank: Immortal3 in Episode 6 (Peak might be incorrect)
```

### Error Handling

- If the `q` parameter is missing, empty, or invalid, the API returns an error message asking for the proper input format.
- If an error occurs while calling the external API, the response returns the error message from the external API.

### Command Setup

Below are examples of how to use the `/checkrank` endpoint in a chatbot command. Replace `<q>` and `<apiKey>` with the appropriate values.

To add a new command in your chatbot, use the following syntax:

=== "Nightbot"

    ```bash
    !addcom !checkrank $(urlfetch https://valo-api.vercel.com/checkrank?q=<q>&apiKey=<apiKey>)
    ```

=== "StreamElements"

    ```bash
    !cmd add !checkrank $(customapi https://valo-api.vercel.com/checkrank?q=<q>&apiKey=<apiKey>)
    ```

=== "Fossabot"

    ```bash
    !addcmd !checkrank $(customapi https://valo-api.vercel.com/checkrank?q=<q>&apiKey=<apiKey>)
    ```

=== "Streamlabs Chatbot"

    ```bash
    !command add !checkrank {readapi.https://valo-api.vercel.com/checkrank?q=<q>&apiKey=<apiKey>}
    ```

To edit an existing command, use the following syntax:

=== "Nightbot"

    ```bash
    !editcom !checkrank $(urlfetch https://valo-api.vercel.com/checkrank?q=<q>&apiKey=<apiKey>)
    ```

=== "StreamElements"

    ```bash
    !cmd edit !checkrank $(customapi https://valo-api.vercel.com/checkrank?q=<q>&apiKey=<apiKey>)
    ```

=== "Fossabot"

    ```bash
    !editcmd !checkrank $(customapi https://valo-api.vercel.com/checkrank?q=<q>&apiKey=<apiKey>)
    ```

=== "Streamlabs Chatbot"

    ```bash
    !command edit !checkrank {readapi.https://valo-api.vercel.com/checkrank?q=<q>&apiKey=<apiKey>}
    ```

### Command Usage

To use the `!checkrank` command in your chatbot, you can type the following command in your chat:

```bash
!checkrank <username> <tag> <region>
```

For example:

```plaintext
!checkrank PlayerName 1234 na
!checkrank PlayerName #1234 na
// Example for a username with spaces
!checkrank Player Name 1234 eu
!checkrank Player Name #1234 eu
```

---

## `/leaderboard-rank` Endpoint

### Description

The `/leaderboard-rank` endpoint retrieves both the current rank and leaderboard position of a specified Valorant player. It provides information on the player's rank, ranking in tier, leaderboard rank, and win rate for the current season.

### HTTP Method

`GET`

### Endpoint

`/leaderboard-rank`

### Request Parameters

| Parameter | Type   | Description                                       | Example                                     |
| --------- | ------ | ------------------------------------------------- | ------------------------------------------- |
| `name`    | string | The Valorant username of the player               | `PlayerName`                                |
| `tag`     | string | The Valorant tag of the player                    | `1234`                                      |
| `region`  | string | The region of the player (e.g., `na`, `eu`, `ap`) | `na`                                        |
| `apiKey`  | string | Your API key obtained from HenrikDev API          | `HDEV-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` |

#### Example Request

```http
GET /leaderboard-rank?name=PlayerName&tag=1234&region=na&apiKey=HDEV-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### Response

- **200 OK**  
  Returns a plain text summary of the player's current rank, leaderboard rank, and win rate.

  **Example Success Response:**
  Current Rank: Immortal3 - 392 RR | Leaderboard Rank: 502 with 71 wins out of 151 games (47.02% wr)

- **400 Bad Request**
  Returns a plain text error message if required parameters (`name`, `tag`, `region`) are missing.

        **Example Error Response:**
        `Error: Missing parameters. Ensure username, tag, and region are provided.`

- **500 Internal Server Error**  
   Returns a plain text error message if an error occurs while fetching data from the external API (e.g., invalid credentials, player not found, etc.).
  **Example Error Response:**
  `Error Message from API`

### Command Setup

Below are examples of how to use the `/leaderboard-rank` endpoint in a chatbot command. Replace `<username>`, `<tag>`, `<region>`, and `<apiKey>` with the appropriate values. This command is best used for players ranked in the Immortal and Radiant tiers.

To add a new command in your chatbot, use the following syntax:

=== "Nightbot"

    ```bash
    !addcom !leaderboardrank $(urlfetch https://valo-api.vercel.com/leaderboard-rank?name=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>)
    ```

=== "StreamElements"

    ```bash
    !cmd add !leaderboardrank $(customapi https://valo-api.vercel.com/leaderboard-rank?name=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>)
    ```

=== "Fossabot"

    ```bash
    !addcmd !leaderboardrank $(customapi https://valo-api.vercel.com/leaderboard-rank?name=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>)
    ```

=== "Streamlabs Chatbot"

    ```bash
    !command add !leaderboardrank {readapi.https://valo-api.vercel.com/leaderboard-rank?name=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>}
    ```

To edit an existing command, use the following syntax:

=== "Nightbot"

    ```bash
    !editcom !leaderboardrank $(urlfetch https://valo-api.vercel.com/leaderboard-rank?name=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>)
    ```

=== "StreamElements"

    ```bash
    !cmd edit !leaderboardrank $(customapi https://valo-api.vercel.com/leaderboard-rank?name=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>)
    ```

=== "Fossabot"

    ```bash
    !editcmd !leaderboardrank $(customapi https://valo-api.vercel.com/leaderboard-rank?name=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>)
    ```

=== "Streamlabs Chatbot"

    ```bash
    !command edit !leaderboardrank {readapi.https://valo-api.vercel.com/leaderboard-rank?name=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>}
    ```

### Command Usage

To use the `!leaderboardrank` command in your chatbot for the configured user, you can type the following command in your chat:

```bash
!leaderboardrank
```

---

## Additional Notes

- The API uses the HenrikDev API internally to fetch the player's rank and leaderboard details.
- Ensure that you have a valid API key to access the endpoints.
- The API responses are in plain text format suitable for chatbot commands.
- The API may return error messages if the required parameters are missing or if there are issues with the external API.
- For detailed error handling and response formats, refer to the specific endpoint documentation.

## External API Used

- [`https://api.henrikdev.xyz/valorant/v2/mmr`](https://docs.henrikdev.xyz/valorant/api-reference/mmr#valorant-v2-mmr-region-name-tag){target='\_blank'} - [`/rank`](#rank-endpoint), [`/checkrank`](#checkrank-endpoint) and [`/leaderboard-rank`](#leaderboard-rank-endpoint) endpoints
- [`https://api.henrikdev.xyz/valorant/v3/leaderboard`](https://docs.henrikdev.xyz/valorant/api-reference/leaderboard#valorant-v3-leaderboard-region-platform){target='\_blank'} - [`/leaderboard-rank`](#leaderboard-rank-endpoint) endpoint
