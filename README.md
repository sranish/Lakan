# Lakan

A video meeting app built with Next.js. Start an instant meeting, schedule one for later, join by link, and watch recordings of past calls.

Named after Lakan, the eccentric strategist from *The Apothecary Diaries*.

## Features

- **Instant meetings**: start a call and share the link
- **Scheduled meetings**: pick a date and time with a description
- **Join by link**: paste a meeting URL to jump in
- **Personal room**: a permanent meeting link tied to your account
- **Upcoming / Previous / Recordings**: browse your calls and play back recordings
- **Pre-join setup**: check your camera and mic before entering
- **Layouts**: switch between grid and speaker views during a call
- **Auth**: every page except sign-in and sign-up needs a login

## Tech stack

| Area      | Tool                                                              |
| --------- | ----------------------------------------------------------------- |
| Framework | [Next.js 14](https://nextjs.org/) (App Router), React 18, TypeScript |
| Video     | [Stream Video React SDK](https://getstream.io/video/)             |
| Auth      | [Clerk](https://clerk.com/)                                       |
| UI        | Tailwind CSS, shadcn/ui (Radix), lucide-react                     |

## Getting started

**Prerequisites:** Node.js 18+, a [Clerk](https://clerk.com) app, and a [Stream](https://getstream.io) app with Video enabled.

```bash
git clone <your-repo-url> lakan
cd lakan
npm install
```

Create `.env.local` in the project root:

```env
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
CLERK_SECRET_KEY=your_clerk_secret_key

NEXT_PUBLIC_STREAM_API_KEY=your_stream_api_key
STREAM_SECRET_KEY=your_stream_secret

NEXT_PUBLIC_BASE_URL=http://localhost:3000
```

Then run:

```bash
npm run dev
```

Open http://localhost:3000.

## Scripts

| Command         | What it does             |
| --------------- | ------------------------ |
| `npm run dev`   | Start the dev server     |
| `npm run build` | Build for production     |
| `npm start`     | Serve the production build |
| `npm run lint`  | Run ESLint               |

## Project structure

```
app/
  (auth)/          sign-in and sign-up pages (Clerk)
  (root)/(home)/   home, upcoming, previous, recordings, personal-room
  (root)/meeting/[id]/  the meeting page (setup screen, then the room)
actions/           server action that creates Stream user tokens
components/        NavBar, SideBar, MeetingRoom, MeetingSetup, CallList, ...
hooks/             useGetCallById, useGetCalls
providers/         StreamClientProvider (Stream client wired to the Clerk user)
middleware.ts      Clerk route protection
```

## How a meeting works

1. Clerk signs the user in. `StreamClientProvider` asks the server action in `actions/stream.actions.ts` for a Stream token and creates the video client.
2. Creating a meeting makes a Stream call whose ID is used in the URL `/meeting/<id>`.
3. The meeting page loads the call, shows `MeetingSetup` (camera and mic preview), then switches to `MeetingRoom` after you join.

## Deployment

Deploy to [Vercel](https://vercel.com/) or any Node host. Set the same environment variables in production, with `NEXT_PUBLIC_BASE_URL` pointing at your deployed domain.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).
