# Discord oAuth2 Guide

A guide that focuses on Discord Authentication, and the inherit advantages / disadvantages for alt:V using different design patterns.

## Introduction

In almost 99% of cases oAuth2 requires the end user to authenticate through an exteral server with discord redirecting the user's bearer token to that server.

However, there are `other` ways to effectively authenticate a user in a non-traditional way.

Let's talk about some of those and their inherit advantages and disadvantages.

## Scoring

- Safety means there's no inherit flaws about using this format
    - Higher is better

- Reliability means how reliable it will be for users using this format
    - Higher is better

- Difficult means how hard it is to implement
    - Lower is better

  
## Regular oAuth2

1. User visits a URL like this:
```ts
const oAuthURL =
    `https://discord.com/api/oauth2/authorize?client_id=` +
    `VAR_CLIENT_ID` +
    `&redirect_uri=` +
    `VAR_REDIRECT_URI` +
    `&response_type=code&scope=identify`;
```
2. User authorizes through Discord
3. The bearer token is passed to the `VAR_REDIRECT_URI`
4. The server takes the bearer token and exchanges it for the user's information
5. You now have identified who the client is.

### Pros

- 100% proof the client is who they say they are
- Seamless experience outside of a game

### Cons

- Requires tabbing out into a browser
- Requires hosting a server with exposed endpoints to ingest the bearer token
- Used inside of a WebView / CEF instance, it requires them to login again
- Requires additional firewall setup
- Requires some form of static endpoint

Difficulty: 5/10

Reliability: 10/10

Safety: 100%

## Discord Bot Message

_The discord bot must be bound to the game server_

1. User joins a server, or starts a game.
2. The client is given a unique identifier / password
3. The bot displays that uid on-screen
4. The client takes the unique identifier / password and is told to DM a bot.
5. User DMs the bot and the message is recieved with the correct identifier / password
6. The bot has now linked the user to the client and can do a login

### Pros

- 100% proof the client is who they say they are
- No exposed endpoints
- Zero configuration for ports, and firewalls

### Cons

- Requires tabbing out into a browser, or opening the Discord Overlay
- Can be a bit confusing for the user the first time they do it
- User must have their DMs open
- User must be in the same server as the Discord Bot

Difficulty: 3/10

Reliability: 10/10

Safety: 100%

## Integrated Client

1. Your software / client must correctly have the Discord SDK Setup (alt:V does this)
2. You use internal APIs to get bearer token
3. Bearer token passed up from client to server
4. Exchange bearer for client information
5. Now know who the client is

### Pros

- Near seamless experience if they have the Discord Application

### Cons

- Does not always work (your experience may vary, and so will your users)
- Requires user to tab out
- Client must have discord application on their desktop
- Client must have application open before opening game, software, etc.
- Does not work in browser

Difficult: 4/10

Reliability: 3/10

Safety: 100%

## Discord RPC (unavailable)

This method requires the Discord Application and `rpc` but is currently closed beta, and unavailable

1. Client-side makes an rpc request to discord application
2. Returns access token
3. Access token exchanged on server-side
4. That's it

### Pros

- Seems simple
- Near seamless experience if they have the Discord Application

### Cons

- Client must have discord application on their desktop
- Client must have application open before opening game, software, etc.
- Client identifier can be spoofed, and is a major vulnerability

Difficult: 3/10

Reliability: 3/10

Safety: 100%
