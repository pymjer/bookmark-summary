Title: GH Archive

URL Source: https://www.gharchive.org/

Markdown Content:
Open-source developers all over the world are working on millions of projects: writing code & documentation, fixing & submitting bugs, and so forth. GH Archive is a project to **record** the public GitHub timeline, **archive it**, and **make it easily accessible** for further analysis.

GitHub provides [15+ event types](https://docs.github.com/en/webhooks-and-events/events/github-event-types), which range from new commits and fork events, to opening new tickets, commenting, and adding members to a project. These events are aggregated into hourly archives, which you can access with any HTTP client:

Query

Command

Activity for 1/1/2015 @ 3PM UTC

`wget https://data.gharchive.org/2015-01-01-15.json.gz`

Activity for 1/1/2015

`wget https://data.gharchive.org/2015-01-01-{0..23}.json.gz`

Activity for all of January 2015

`wget https://data.gharchive.org/2015-01-{01..31}-{0..23}.json.gz`

Each archive contains JSON encoded events as reported by the GitHub API. You can download the raw data and apply own processing to it - e.g. write a custom aggregation script, import it into a database, and so on! An example Ruby script to download and iterate over a single archive:
