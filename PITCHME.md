# Master

---

### What, Why, How, When
- A project that maintains the master data of all of our channels and all of its metadata
- To have it in the most suitable form
- By updating all data as much as we can
- As soon as we detect that a change happened

---

### API

---

#### Routes

- **GET** `/videos` - Used by Tunes to get latest data of a video. It grabs the latest data of the given video IDs and updates the database.
- **POST** `/import_copyright` - Used by Frnky to save the downloaded report of copyrighted videos.

---

### Jobs

---

#### bi-hourly.sh

1. <span style="color: #e49436">import_ytfreedom</span> - Inserts new channels from *ytfreedom.channels* to *master.mcn_channels*
2. <span style="color: #e49436">new_partners</span> - Calls *foxy.crawl*.
3. <span style="color: #e49436">non_partners</span> - Calls *foxy.update_non_partners*.
4. <span style="color: #e49436">partners</span> - Calls *foxy.update_partners*.
5. <span style="color: #e49436">analytics</span> - Calls *foxy.update_analytics*.
6. <span style="color: #e49436">videos</span> - Calls *foxy.replace_videos*.

> All of these jobs are called with *cluster.js*. Most of them has a specfic batch size which directly relates to the server's processing powers.

----

#### Daily tasks

1. Update last 30/60/90 days stats of those that were not updated by the API using the archive
2. Set all flags to 0
3. Save the day's earnings to *channel_earnings* table
4. Save the day's stats to *channel_stats* table
5. Reset all API keys and Google projects

---

#### Severus
- Takes a snapshot of the stats of the top networks
- Absolutely no idea why this is on this repo

---

### History / Trivias
- Started Oct 2014 from a dev's idea
- `cuddle` was made for this project due to the massive API calls
- `anytv-node-mysql` was born from this project
- The top 1 contributor has 407 commits and the top 2 has 23 commits
