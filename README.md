# The Matchups

**Know the matchup before the game starts.** → [thematchups.com](https://thematchups.com)

The Matchups is a sports matchup explorer for the NFL and MLB. For every game it pulls together what a fan
actually wants to know before kickoff or first pitch: who's in and who's out, what's changed since last week,
where each team has the edge, and how the numbers compare across the league. Built-in AI analyst **Scout**
answers questions about any game using the same numbers on the page.

> The source code is private. This page describes the product and how it's built.

---

## What's on the site

### NFL
- **Game pages** that open with *This week's outlook*: week-to-week changes first (a starting quarterback
  trending back from injury, players newly on the report, recent form against the season baseline), then the
  season matchup.
- **Matchup insights:** short, stat-backed cards on each game's biggest offense-vs-defense mismatches, the
  quarterback duel and the division race.
- **Team vs team** comparisons in a Simple view (points, yards, turnovers) and an Advanced view (EPA, success
  rate, explosive plays, third down, red zone), every number ranked 1–32 with plain-English explanations.
- **Coaching tendencies** for each staff: play-action, motion, blitz rate, fourth-down aggressiveness and more.
- **Availability tracking** that compares each player's status in his team's last game with this week's
  practice reports, so a returning star shows up before the final injury report does.
- **Kickoff weather**, series history since 1999, and league-wide **Team Stats, Player Stats, Injuries and
  Stadium** pages.
- **Player pages** with bio, draft details, this season's numbers ranked against the league, game logs and
  career tables with career highs marked.

### MLB
- **Daily game dashboard:** lineups, starting pitchers, batter-vs-pitcher history, ballpark details and
  game-time weather with wind direction relative to the field.
- **Postseason-aware stats:** regular season, postseason and head-to-head numbers are kept separate and
  labeled everywhere, including in what the AI sees.
- **Team comparisons, league leaders, injured lists, ballparks and player pages.**

### Scout, the AI analyst
A chat panel on every game page. Ask "Is Lamar playing Sunday?" or "Where does Buffalo have the edge?" and
Scout answers from a briefing the server builds for that game, so its answers match the page. It's built to
say what it doesn't know rather than guess, to separate small samples from real trends, and to stay away from
betting picks and score predictions.

---

## How it's built

| Area | Approach |
|---|---|
| **App** | Python / Flask, one module per sport behind a shared layout; server-rendered pages with lightweight vanilla JavaScript. |
| **NFL data** | Open nflverse datasets (play-by-play, weekly stats, injury reports, FTN charting), turned into per-team efficiency numbers, league ranks, availability trends and recent form. |
| **MLB data** | MLB Stats API with caching and retries; venue data merged with hand-written ballpark notes; weather from Open-Meteo. |
| **AI** | Anthropic's Claude for Scout and the one-tap matchup analysis, with server-built game briefings, input limits and per-visitor rate limiting. |
| **Quality** | An offline test suite that replays recorded real-world data (no network needed), plus an **AI evaluation suite**: dozens of realistic fan questions across both sports, graded by rule checks and an AI judge, run on every change to the AI code. |
| **Hosting** | Render, deployed from GitHub on every push, custom domain with HTTPS, health checks, privacy-friendly analytics and a built-in feedback channel. |

### A few engineering details
- **Every number has a rank and a plain-English explanation.** Hover any stat for what it means; league
  ranks and averages sit next to each value so fans can tell good from great.
- **The AI is measured, not guessed at.** Prompt and model changes are scored against the eval suite before
  they ship; one round of fixes took NFL Scout from 84% to 92% on its cases while halving response time.
- **Built to hold up on a small server:** in-memory caching of large datasets, background warm-up of slow
  files, and memory-conscious loading of only the columns each page needs.
- **Designed for phones too:** every page was reviewed for readability at phone and laptop widths.

---

## Data & credits
NFL data from [nflverse](https://github.com/nflverse) (CC BY 4.0); charting data from FTN Data via nflverse
(CC BY-SA 4.0). MLB data from the MLB Stats API. Weather by [Open-Meteo](https://open-meteo.com) (CC BY 4.0).
The Matchups is an independent project and is not affiliated with or endorsed by the NFL or MLB.

## Contact
Built by Hunter Cordova ([@Hcordova2018](https://github.com/Hcordova2018)). Questions or feedback: use the
**Feedback** button on [thematchups.com](https://thematchups.com).
