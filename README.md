<div align="center">

# httpSMS

<img src="https://readme-typing-svg.demolab.com?font=Inter&weight=700&size=28&duration=2800&pause=700&color=0EA5E9&center=true&vCenter=true&width=900&lines=Send+%2B+Receive+SMS+via+Android;Programmatic+SMS+API;Simple+HTTP+Integration;Open+source+messaging+bridge" alt="Typing SVG" />

<p>
  <img src="https://komarev.com/ghpvc/?username=mafzalkalwardev&label=Project%20Views&color=0ea5e9&style=for-the-badge" alt="Project views" />
  <img src="https://img.shields.io/github/stars/mafzalkalwardev/httpsms?style=for-the-badge&color=22c55e" alt="GitHub stars" />
  <img src="https://img.shields.io/github/forks/mafzalkalwardev/httpsms?style=for-the-badge&color=f59e0b" alt="GitHub forks" />
  <img src="https://img.shields.io/github/license/mafzalkalwardev/httpsms?style=for-the-badge&color=64748b" alt="License" />
</p>

<p>
  <img src="https://skillicons.dev/icons?i=go,android" alt="Skill icons" />
</p>

<p>
  <strong>Send and receive SMS messages using your Android phone programmatically via a simple HTTP API.</strong>
</p>

</div>

---

## Contribution Snake

<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/mafzalkalwardev/google-voice-dispatch-agent/output/github-contribution-grid-snake-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/mafzalkalwardev/google-voice-dispatch-agent/output/github-contribution-grid-snake.svg">
    <img alt="Contribution snake" src="https://raw.githubusercontent.com/mafzalkalwardev/google-voice-dispatch-agent/output/github-contribution-grid-snake.svg">
  </picture>
</div>

---

## SEO Keywords

`SMS API` `Android SMS` `httpSMS` `Go`

---

## Why?

I'm originally from Cameroon and I wanted an automated way to send and receive SMS messages using an API.
Unfortunately many countries don't support the ability to buy virtual phone numbers, and I could not find a good ready-made
solution that could help me send/receive SMS messages using a mobile phone using an intuitive http API.

## Web UI

The web interface https://httpsms.com is built using [Nuxt](https://nuxtjs.org/) and [Vuetify](https://vuetifyjs.com/en/).
It is hosted as a single page application on firebase. The source code is in the [web](./web) directory

## API

The API https://api.httpsms.com is built using [Fiber](https://gofiber.io/), Go and [CockroachDB](https://www.cockroachlabs.com/) for the database.
It rus as a serverless application on Google Cloud Run. The API documentation can be found here https://api.httpsms.com/index.html

```go
// Sending an SMS Message using Go
client := htpsms.New(htpsms.WithAPIKey(/* API Key from https://httpsms.com/settings */))

client.Messages.Send(context.Background(), &httpsms.MessageSendParams{
    Content: "This is a sample text message",
    From:    "+18005550199",
    To:      "+18005550100",
})
```

## Android App

[The Android App](https://apk.httpsms.com/HttpSms.apk) is a native application built using Kotlin with material design principles.
This app must be installed on an Android phone before you can start sending and receiving SMS messages.

[<img src=".github/ghbadge.png" alt="Get it on GitHub" height="80">](https://github.com/NdoleStudio/httpsms/releases/)

## Chat/forum

There are a few ways to get in touch with me and/or the rest of the community. Feel free to use any of these methods. Whatever
works best for you:

- [Discord server](https://discord.gg/kGk8HVqeEZ) - direct chat with the community
- [GitHub issues](https://github.com/NdoleStudio/httpsms/issues) - questions, features, bugs

## Features

### End-to-end Encryption

You can encrypt your messages end-to-end ysubg the military grade [AES-256 encryption](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
algorithm. Your encryption key is stored only on our mobile phone so the even the server won't have any way to view the
content of your SMS messages which are sent and received on your Android phone.

### Webhook

If you want to build advanced integrations, we support webhooks. The httpSMS platform can forward SMS messages received
on the android phone to your server using a callback URL which you provide.

### Back Pressure

In-order not to abuse the SMS API on android, you can set a rate limit e.g 3 messages per minute. Such that even if you
call the API to send messages to 100 people, It will only send the messages at a rate of 3 messages per minute.

### Message Expiration

Sometimes it happens that the phone doesn't get the push notification in time and I can't send the SMS message. It is
possible to set a timeout for which a message is valid and if a message becomes expired after the timeout elapses, you
will be notified.

## API Clients

- [x] Go: https://github.com/NdoleStudio/httpsms-go
- [x] JavaScript/TypeScript: https://github.com/NdoleStudio/httpsms-node

## Flows

### Sending an SMS Message

```mermaid
sequenceDiagram
User->>+httpSMS API: Call /v1/messages/send API
httpSMS API-->>+Push Queue: Schedule notification about new message
httpSMS API-->>-User: Respond with 202 (Accepted)
Push Queue-->>+httpSMS API: [Async] Send notification request
httpSMS API-->>-Android App: Send push notification about new message
Android App-->>httpSMS API: [Async] Fetch message
Android App-->>Android App: Send Message using Android SMS API
Android App-->>httpSMS API: [Async] Send result of sending SMS
Android App-->>httpSMS API: [Async] Send Delivery Report
```

## Self Host Setup - Docker

### 1. Setup Firebase

- The httpSMS application uses [firebase cloud messaging](https://firebase.google.com/docs/cloud-messaging) for sending push notifications to your Android phone to trigger an SMS message to be sent out.
  Visit the [firebase console](https://console.firebase.google.com/) and create a new project and follow the [steps here](https://firebase.google.com/docs/web/setup#register-app) to get your firebase web SDK config credentials.
  For example, I created a firebase project called `httpsms-docker` and this is my web SDK configuration

```js
const firebaseConfig = {
  apiKey: "AIzaSyAKqPvj51igvvNNcRtxxxxx",
  authDomain: "httpsms-docker.firebaseapp.com",
  projectId: "httpsms-docker",
  storageBucket: "httpsms-docker.appspot.com",
  messagingSenderId: "668063041624",
  appId: "1:668063041624:web:29b9e3b702796xxxx",
  measurementId: "G-18VRYL2xxxx",
};
```

- Enable `Email/Password` sign-in in the [Firebase console](https://console.firebase.google.com/u/0/), open the **Authentication** section. On the Sign in method tab, enable the `Email/password` sign-in method and click `Save`.
  - The firebase `email/password` sign-in method has [a bug](https://github.com/firebase/firebaseui-web/issues/1040) which prevents you from signing in. The work around right now is to [disable email enumeration protection](https://cloud.google.com/identity-platform/docs/admin/email-enumeration-protection#disable) on the firebase console.
- Generate your firebase service account credentials by following the [steps here](https://firebase.google.com/docs/admin/setup#initialize_the_sdk_in_non-google_environments) and save the credentials in a file called `firebase-credentials.json` we will use this file to authenticate with the firebase admin SDK.
- Generate your Android `google-services.json` file using [the instructions here](https://support.google.com/firebase/answer/7015592?hl=en#android&zippy=%2Cin-this-article) we will use it letter to configure the android app.

### 2. Setup SMTP Email service

The httpSMS application uses [SMTP](https://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol) to send emails to users e.g. when your Android phone has been offline for a long period of time.
You can use a service like [mailtrap](https://mailtrap.io/) to create an SMTP server for development purposes.

### 3. Setup Cloudflare Turnstile

The message search route (`/v1/messages/search`) is protected by a [Cloudflare Turnstile](https://developers.cloudflare.com/turnstile/get-started/) captcha to prevent abuse. You need to set up a Turnstile widget for the search messages feature to work.

1. Go to the [Cloudflare dashboard](https://dash.cloudflare.com/) and navigate to **Turnstile**.
2. Add a new site and configure it for your self-hosted domain (e.g., `localhost` for local development).
3. Note down the **Site Key** and **Secret Key** — you will need them for the frontend and backend environment variables respectively.

### 4. Download the code

Clone the httpSMS GitHub repository

```bash
git clone https://github.com/NdoleStudio/httpsms.git
```

### 5. Setup the environment variables

- Copy the `.env.docker` file in the `web` directory into `.env`

```bash
cp web/.env.docker web/.env
```

- Update the environment variables in the `.env` file in the `web` directory with your firebase web SDK configuration in step 1 above

```dotenv
FIREBASE_API_KEY=
FIREBASE_AUTH_DOMAIN=
FIREBASE_PROJECT_ID=
FIREBASE_STORAGE_BUCKET=
FIREBASE_MESSAGING_SENDER_ID=
FIREBASE_APP_ID=
FIREBASE_MEASUREMENT_ID=

# Cloudflare Turnstile site key from step 3
CLOUDFLARE_TURNSTILE_SITE_KEY=
```

- Copy the `.env.docker` file in the `api` directory into `.env`

```bash
cp api/.env.docker api/.env
```

- Update the environment variables in the `.env` file in the `api` directory with your firebase service account credentials, SMTP server details, and Cloudflare Turnstile secret key.

```dotenv
# SMTP email server settings
SMTP_USERNAME=
SMTP_PASSWORD=
SMTP_HOST=
SMTP_PORT=

# Firebase service account credentials
FIREBASE_CREDENTIALS=

# This is the `projectId` from your firebase web config
GCP_PROJECT_ID=

# Cloudflare Turnstile secret key from step 3
CLOUDFLARE_TURNSTILE_SECRET_KEY=
```

- Don't bother about the `EVENTS_QUEUE_USER_API_KEY` and `EVENTS_QUEUE_USER_ID` settings. We will set that up later.

### 6. Build and Run

- Build and run the API, the web UI, database and cache using the `docker-compose.yml` file. It takes a while for build and download all the docker images.
  When it's finished, you'll be able to access the web UI at http://localhost:3000 and the API at http://localhost:8000

```bash
docker compose up --build
```

### 7. Create the System User

- The application uses the concept of a system user to process events async. You should manually create this user in `users` table in your database. Make sure you use the same `id` and `api_key` as the `EVENTS_QUEUE_USER_ID`, and `EVENTS_QUEUE_USER_API_KEY` in your `.env` file.

  ```SQL
  INSERT INTO users (id, api_key, email ) VALUES ('your-system-user-id', 'your-system-api-key', 'system@domain.com');
  ```

> [!IMPORTANT]
> Restart your API docker container after modifying `EVENTS_QUEUE_USER_ID`, and `EVENTS_QUEUE_USER_API_KEY` in your `.env` file so that the httpSMS API can pick up the changes.

### 8. Build the Android App.

- Before building the Android app in [Android Studio](https://developer.android.com/studio), you need to replace the `google-services.json` file in the `android/app` directory with the file which you got from step 1. You need to do this for the firebase FCM messages to work properly.

## Integration Testing

The project includes end-to-end integration tests that validate the complete SMS send/receive lifecycle. Tests run the full stack (API, PostgreSQL, Redis) in Docker alongside a phone emulator that simulates an Android device.

📖 **Full documentation:** [`tests/README.md`](tests/README.md)

**Quick run:**

```bash
cd tests
bash generate-firebase-credentials.sh
export FIREBASE_CREDENTIALS=$(jq -c . firebase-credentials.json)
docker compose up -d --build --wait
docker compose wait seed && sleep 2
go test -v -timeout 120s ./...
docker compose down -v
```

Integration tests also run automatically in CI on every push/PR to `main`.

## License

This project is licensed under the GNU AFFERO GENERAL PUBLIC LICENSE Version 3 - see the [LICENSE](LICENSE) file for details
