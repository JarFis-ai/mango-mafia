# Mango Mafia - case study

**[Read the full case study →](https://jarfis-ai.github.io/mango-mafia/)**

Mango Mafia is Mafia for an ESL classroom. The teacher runs it off one projector, the
students have no devices, and the app is the moderator - it deals the roles, calls each
one in turn, resolves the night, counts the votes, and prints the exact sentence the
teacher should say next.

It is built for hagwons and academies in Korea: 4 to 20 students, typically 6 to 12,
roughly 7 to 16 years old.

This repository is the case study and screenshots. The application source is private.

---

## The problem that shapes everything

The app holds every secret in the game, on a screen all twenty players are looking at.

When the teacher clicks a card to register the Mafia's target, nothing may persist
visibly - no highlight, no checkmark, no colour change. A prompt as innocent as
"waiting for Mafia (2 remaining)" leaks how many Mafia are still alive. Information that
belongs to one role may appear only during that role's turn.

So "what may be drawn right now" is a pure function returning a value, not a conditional
buried in JSX - which makes it something a test can hold. The leak audits play two games
that differ only in hidden state (Doctor alive or dead, target chosen or not, self-save
spent or not) and require the rendered output to come out identical, turn for turn.

## Built with

Next.js 16 (App Router) · React 19 · TypeScript · Tailwind v4 · Vitest · Vercel

The entire live session is one `useReducer` in the browser tab - roster, roles, night
actions, votes and phase. No server-side session, no realtime, no student clients, and no
network call during a lesson.

## Where it stands

| | |
|---|---|
| Tests | 459 passing across 38 files |
| Full games played inside the suite | 7 |
| Roles | 15 designed, 4 shipped free, 11 drawn and specified |
| Character files | 42 |
| Playthrough rounds fed back in | 4 |

## Three things a passing test suite could not tell me

1. **A projector cannot render black.** The app was near-black and looked fine on every
   monitor it was built on. In a lit classroom a projector renders black as *no light*,
   which is the same washed-out grey as the wall - so the Mafia mango in his black suit
   disappeared into the background. Every surface moved off black, the ink now follows the
   surface, and a test reads the stylesheet and enforces the contrast ratios.
2. **A driven browser found what static markup could not**: an OUT badge painted over by
   the art slot below it, and a name wrapping across two lines on the reveal splash.
3. **"The roles are not randomising"** was reported from a real lesson, investigated
   across 12,000 deals, and turned out to be correct randomness. Half a class of eight are
   Citizens, so several students genuinely do draw the same role twice in a row. Written
   down so nobody spends a day on it again.

---

Built by [Jacobus Barnard](https://github.com/JarFis-ai), a teacher and developer in
Seoul. Also: [59 Seconds](https://jarfis-ai.github.io/), a live ESL speaking game.
