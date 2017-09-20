# Master

---

### What, Why, How, When
- A project that maintains the master data of all of our channels (linked, not linked, previously added)
- To be the most accesible and most reliable source after the YouTube API
- By actively updating all the data as much as we can
- As soon as we detect that a change happened

---

### History / Trivias
- Started Oct 2014
- `cuddle` was made for this project due to the massive API calls
- `anytv-node-mysql` was born from this projects

---

### API

---

#### Routes

- `GET /videos` - Used by Tunes to get latest data of a video
- `POST /import_copyright` - Used by Frnky to save the downloaded report of copyrighted videos we have

---

### Jobs

---

#### bi-hourly.sh

1. `import_ytfreedom` - Inserts new channels from *ytfreedom.channels* to *master.mcn_channels*
2. `new_partners` - Calls *foxy.crawl*. Grabs all our partners from YouTube API then detect which channels are new, moved and unlinked.
3. `non_partners` - Calls *foxy.update_non_partners*.
4. `partners` - Calls *foxy.update_partners*.
5. `analytics` - Calls *foxy.update_analytics*.
6. `videos` - Calls *foxy.replace_videos*.

All of these jobs are called together with *cluster.js* which helps in doing things faster.

----

#### daily tasks

1. update last 30 days stats of those that were not updated by the API
2. set all flag to 0
3. archive day earnings to *channel_earnings* table
4. archive day stats to *channel_stats* table
5. reset keys

---

#### severus
- takes a snapshot of the stats of the top networks
- no idea why this is on this repo
