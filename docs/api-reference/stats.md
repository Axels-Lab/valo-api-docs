# Stats Related API Endpoints

This section provides detailed information about the `/stats`, `/checkstats` and `/party-members` endpoints, which are used to retrieve player statistics and party members of a specified Valorant player.

## `/stats` Endpoint

### Description

The `/stats` endpoint is used to retrieve the player statistics of a specified Valorant player. It returns a summary of the player's stats for the last match and an aggregate summary of their last available 5 matches.

### HTTP Method

`GET`

### Endpoint

`/stats`

### Request Parameters

| Parameter | Type   | Required | Description                                          |
| --------- | ------ | -------- | ---------------------------------------------------- |
| username  | string | Yes      | The Valorant player name to retrieve statistics for. |
| tag       | string | Yes      | The Valorant player tag to retrieve statistics for.  |
| region    | string | Yes      | The region of the Valorant player.                   |
| apiKey    | string | Yes      | Your API key obtained from HenrikDev API.            |

**_Note: `tag` should be provided without the `#` symbol at the start._**

#### Example Request

```plaintext
GET /stats?username=playername&tag=1234&region=na&apiKey=yourapikey
```

### Response

- **200 OK**

  Returns a summary of the player's stats for the last match and an aggregate summary of their last available matches.

      **Example Success Response:**
      ```plaintext
      Last Match Stats: Agent – Jett | K/D/A – 20/10/5 | ACS – 250 | ADR – 150 | DDΔ – 100 | HS% – 25.00% | Combined Stats (Last 5 Matches): K/D/A – 100/50/25 | ACS – 245 | ADR – 148 | DDΔ – 90 | HS% – 24.50%
      ```

      **Explanation of the Fields:**

          - Last Match Stats:
              - Agent: The agent the player used in the last match.
              - K/D/A: The player's kills, deaths, and assists in the last match.
              - ACS: Average combat score for the player in the last match.
              - ADR: Average damage per round in the last match.
              - DDΔ (Damage Delta): Difference between damage made and damage received per round.
              - HS%: Headshot percentage for the last match.
          - Combined Stats:
              - Shows aggregated statistics across all available matches.
              - K/D/A: Combined kills, deaths, and assists from the last available matches.
              - ACS: Average combat score across the last available matches.
              - ADR: Average damage per round across all matches.
              - DDΔ (Damage Delta): Combined damage delta across all matches.
              - HS%: Average headshot percentage across all matches.

- **400 Bad Request**

  Returns an error if required parameters (username, tag, or region) are missing or invalid.

  **Example Error Response:**
  Error: Missing parameters. Ensure username, tag, and region are provided.

- **500 Internal Server Error**

  Returns an error if there is an issue fetching or parsing the match data.

  **Example Error Response:**
  Failed to parse match stats: `<Error Message from API>`

### Command Setup

Below are examples of how to use the `/stats` endpoint in a chatbot command. Replace `<username>`, `<tag>`, `<region>`, and `<apiKey>` with the appropriate values.

To add a new command in your chatbot, use the following syntax:

=== "Nightbot"

    ```bash
    !addcom !stats $(urlfetch https://valo-api.vercel.app/stats?username=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>)
    ```

=== "StreamElements"

    ```bash
    !cmd add !stats $(customapi https://valo-api.vercel.app/stats?username=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>)
    ```

=== "Fossabot"

    ```bash
    !addcmd !stats $(customapi https://valo-api.vercel.app/stats?username=<username>&tag=<tag>&region=<region>&apiKey)

=== "Streamlabs Chatbot"

    ```bash
    !command add !stats {readapi.https://valo-api.vercel.app/stats?username=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>}
    ```

To edit an existing command, use the following syntax:

=== "Nightbot"

    ```bash
    !editcom !stats $(urlfetch https://valo-api.vercel.app/stats?username=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>)
    ```

=== "StreamElements"

    ```bash
    !cmd edit !stats $(customapi https://valo-api.vercel.app/stats?username=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>)
    ```

=== "Fossabot"

    ```bash
    !editcmd !stats $(customapi https://valo-api.vercel.app/stats?username=<username>&tag=<tag>&region=<region>&apiKey)
    ```

=== "Streamlabs Chatbot"

    ```bash
    !command edit !stats {readapi.https://valo-api.vercel.app/stats?username=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>}
    ```

### Command Usage

To use the `!stats` command in your chatbot for the configured user, you can type the following in your chat:

```plaintext
!stats
```

## `/checkstats` Endpoint

### Description

The `/checkstats` endpoint is similar to the `/stats` endpoint but it takes in query parameters in a different format. It is used to retrieve the player statistics of a specified Valorant player. It returns a summary of the player's stats for the last match and an aggregate summary of their last available 5 matches.

### HTTP Method

`GET`

### Endpoint

`/checkstats`

### Request Parameters

| Parameter | Type   | Description                                                                                | Example                                     |
| --------- | ------ | ------------------------------------------------------------------------------------------ | ------------------------------------------- |
| `q`       | String | A single string that combines the player's username, tag, and region, separated by spaces. | `PlayerName 1234 na`                        |
| `apiKey`  | String | Your API Key for HenrikDev API.                                                            | `HDEV-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` |

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

  Returns a summary of the player's stats for the last match and an aggregate summary of their last available matches.

  - If valid data is found, the response will include:
    - The last match stats of the player.
    - A summary of combined stats for all available matches.

  Response Example:

  ```plaintext
  Last Match Stats: Agent – Jett | K/D/A – 20/10/5 | ACS – 245 | ADR – 145 | DDΔ – 15 | HS% – 22.45% || Combined Stats (Last 5 Matches): K/D/A – 80/50/20 | ACS – 230 | ADR – 140 | DDΔ – 10 | HS% – 21.34%
  ```

- **400 Bad Request**

  Returns an error if required parameters are missing or invalid.

  Error Response Example:

  ```plaintext
  Error: Missing or invalid parameters. Ensure username, tag, and region are provided.
  ```

- **500 Internal Server Error**

  Returns an error if there is an issue fetching or parsing the match data.

  Error Response Example:

  ```plaintext
  Failed to parse match stats: <Error Message from API>
  ```

### Command Setup

Below are examples of how to use the `/checkstats` endpoint in a chatbot command. Replace `<q>` and `<apiKey>` with the appropriate values.

To add a new command in your chatbot, use the following syntax:

=== "Nightbot"

    ```bash
    !addcom !checkstats $(urlfetch https://valo-api.vercel.app/checkstats
    ?q=<q>&apiKey=<apiKey>)
    ```

=== "StreamElements"

    ```bash
    !cmd add !checkstats $(customapi https://valo-api.vercel.app/checkstats
    ?q=<q>&apiKey=<apiKey>)
    ```

=== "Fossabot"

    ```bash
    !addcmd !checkstats $(customapi https://valo-api.vercel.app/checkstats
    ?q=<q>&apiKey=<apiKey>)
    ```

=== "Streamlabs Chatbot"

    ```bash
    !command add !checkstats {readapi.https://valo-api.vercel.app/checkstats
    ?q=<q>&apiKey=<apiKey>}
    ```

To edit an existing command, use the following syntax:

=== "Nightbot"

    ```bash
    !editcom !checkstats $(urlfetch https://valo-api.vercel.app/checkstats
    ?q=<q>&apiKey=<apiKey>)
    ```

=== "StreamElements"

    ```bash
    !cmd edit !checkstats $(customapi https://valo-api.vercel.app/checkstats
    ?q=<q>&apiKey=<apiKey>)
    ```

=== "Fossabot"

    ```bash
    !editcmd !checkstats $(customapi https://valo-api.vercel.app/checkstats
    ?q=<q>&apiKey=<apiKey>)
    ```

=== "Streamlabs Chatbot"

    ```bash
    !command edit !checkstats {readapi.https://valo-api.vercel.app/checkstats
    ?q=<q>&apiKey=<apiKey>}
    ```

### Command Usage

To use the `!checkstats` command in your chatbot, you can type the following in your chat:

```plaintext
!checkstats <username> <tag> <region>
```

For example:

```plaintext
!checkstats PlayerName 1234 na
!checkstats PlayerName #1234 na
// Example for a username with spaces
!checkstats Player Name 1234 eu
!checkstats Player Name #1234 eu
```

---

## `/party-members` Endpoint

### Description

The `/party-members` endpoint is used to retrieve the party members of a specified Valorant player. It returns a list of the player's party members in the last match.

### HTTP Method

`GET`

### Endpoint

`/last-match/party-members`

### Request Parameters

| Parameter | Type   | Description                                             | Example                                                 |
| --------- | ------ | ------------------------------------------------------- | ------------------------------------------------------- |
| username  | string | The Valorant player name to retrieve party members for. | `playername`                                            |
| tag       | string | The Valorant player tag to retrieve party members for.  | `1234`                                                  |
| region    | string | The region of the Valorant player.                      | `na`                                                    |
| apiKey    | string | Your API key obtained from HenrikDev API.               | `HDEV-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`             |


**_Note: `tag` should be provided without the `#` symbol at the start._**

#### Example Request

```plaintext
GET /party-members?username=playername&tag=1234&region=na&apiKey=HDEV-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### Response

- **200 OK**

  Returns a list of the player's party members in the last match.

      **Example Success Response:**
      ```plaintext
      Party Members: Player1, Player2, Player3
      // If the player was solo in the last match:
      No party members found
      ```

- **400 Bad Request**

  Returns an error if required parameters (username, tag, or region) are missing or invalid.

        **Example Error Response:**
        Error: Missing parameters. Ensure username, tag, and region are provided.

- **500 Internal Server Error**

  Returns an error if there is an issue fetching or parsing the match data.

        **Example Error Response:**
        Failed to parse party members: `<Error Message from API>`

### Command Setup

Below are examples of how to use the `/last-match/party-members` endpoint in a chatbot command. Replace `<username>`, `<tag>`, `<region>`, and `<apiKey>` with the appropriate values.

To add a new command in your chatbot, use the following syntax:

=== "Nightbot"

    ```bash
    !addcom !party $(urlfetch https://valo-api.vercel.app/last-match/party-members?username=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>)
    ```

=== "StreamElements"

    ```bash
    !cmd add !party $(customapi https://valo-api.vercel.app/last-match/party-members?username=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>)
    ```

=== "Fossabot"

    ```bash
    !addcmd !party $(customapi https://valo-api.vercel.app/last-match/party-members?username=<username>&tag=<tag>&region=<region>&apiKey)
    ```

=== "Streamlabs Chatbot"

    ```bash
    !command add !party {readapi.https://valo-api.vercel.app/last-match/party-members?username=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>}
    ```

To edit an existing command, use the following syntax:

=== "Nightbot"

    ```bash
    !editcom !party $(urlfetch https://valo-api.vercel.app/last-match/party-members?username=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>)
    ```

=== "StreamElements"

    ```bash
    !cmd edit !party $(customapi https://valo-api.vercel.app/last-match/party-members?username=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>)
    ```

=== "Fossabot"

    ```bash
    !editcmd !party $(customapi https://valo-api.vercel.app/last-match/party-members?username=<username>&tag=<tag>&region=<region>&apiKey)
    ```

=== "Streamlabs Chatbot"

    ```bash
    !command edit !party {readapi.https://valo-api.vercel.app/last-match/party-members?username=<username>&tag=<tag>&region=<region>&apiKey=<apiKey>}
    ```

### Command Usage

To use the `!party` command in your chatbot for the configured user, you can type the following in your chat:

```plaintext
!party
```

---

## Additional Notes

- The API uses the HenrikDev API internally to fetch the player's stats and details.
- Ensure that you have a valid API key to access the endpoints.
- The API responses are in plain text format suitable for chatbot commands.
- The API may return error messages if the required parameters are missing or if there are issues with the external API.
- For detailed error handling and response formats, refer to the specific endpoint documentation.

## External API Used

- [https://api.henrikdev.xyz/valorant/v3/matches](https://docs.henrikdev.xyz/valorant/api-reference/matchlist#valorant-v3-matches-region-name-tag){target="\_blank"} - HenrikDev API for fetching Valorant match data. Used in the [`/checkstats`](#checkstats-endpoint), [`/stats`](#stats-endpoint) and [`/party-members`](#party-members-endpoint) endpoints.
