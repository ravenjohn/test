# Foxy

---

#### What
- A **node module** that keeps YouTube data and metadata up to date on a database

#### Why
- To have the latest data available on a database instead of querying YouTube API

#### How
- By giving the data to a <span style="color: #e49436">**plumber**</span> which blasts the data to a system of connected <span style="color: #e49436">**pipes**</span> starting from the <span style="color: #e49436">**source**</span>

---

### Source
- A special type of pipe that transforms raw data to a known form that will be passed on to the next pipe
- Current available source pipes:
  1. ChannelList - Converts an array of objects to an array of Channel objects.
  2. Crawler - Converts a `cms` to an array of channels that belongs to it.
  3. VideoList - Converts an array of objects to an array of Video objects. Currently not being used.

---

### Plumber
- An object that forms a system by connecting a **source** to any number of **pipes**
- The order of pipes connected are extremely important to make everything work as expected
- Knows the state of every pipe connected (how many are [done | left])
- Responsible for blasting out the whole system

---

### Pipe
- An object that *receives* and *passes* an array of data indefinitely
- Can have a filter where those that fail will be immediately passed
- Has an objective that should be fulfilled
- The size of an array received will not always be the size of the array passed but all the data will still be passed. If it received an array with a size of 50, it can be passed as 50 arrays with a size of 1. The size of array passed will heavily depend on the pipe's objective.
- Can be configured via config.
- Currently there are 16 available pipes. Here are some notable pipes:
  1. Grouper - for regulating the size of arrays that goes through the system because some pipes perform better when they receive a larger/smaller size
  2. TerminationHistory - completely useless if `Terminator` pipe is not connected ahead of it
  3. Updater - should be connected last since it is the one responsible for updating the database
  4. VideoIDGetter - completely alters the data that goes through the system, from channels objects to video objects.

---

## Pre-built systems
- Plumbers that have a system that are readily available

---

#### foxy.crawl('cms_here')
- Uses `Crawler` as *source*
- Has only one connected pipe, the `Inserter`, responsible for inserting the new partners, detecting channels that moved cms and recording the changes to `cms_changes` table
- Currently used by anytv/master

---

#### foxy.update_non_partners([array of channel objects])
- Uses `ChannelList` as *source*
- Has 7 connected pipes: Syncr, YTChannel, Terminator, Mapper, TerminationHistory, Grouper and of course the Updater.
- Currently used by anytv/master

---

#### foxy.update_partners([array of channel objects])
- Uses `ChannelList` as *source*
- Has 9 connected pipes: Syncr, YTChannel, Terminator, Mapper, TerminationHistory, Grouper, Unlinker, Grouper, Updater.
- Currently used by anytv/master and anytv/frnky.

---

#### foxy.update_analytics([array of channel objects])
- Uses `ChannelList` as *source*
- Has 7 connected pipes: Analytics(30), Analytics(90), Analytics(lifetime), CLD, Mapper, Grouper, Updater.
- Currently used by anytv/master.

---

#### foxy.collect_data_for_review([array of channel objects])
- Uses `ChannelList` as *source*
- Has 7 connected pipes: YTChannel, LastUploadDate, Mapper, Updater, **VideoIDGetter**, YTVideo, VideoUpserter.
- Currently used by anytv/channel-analyzer

---

#### foxy.replace_videos([array of channel objects])
- Uses `ChannelList` as *source*
- Has 7 connected pipes: VideoIDGetter, Grouper, YTVideo, CLD, Grouper, VideoDetailsCollector, VideoUpserter.
- Currently used by anytv/master

---

### Quirks
- Foxy still cannot use multipe types of authentication in a system.
- Foxy exits the program when there are no more api keys or google projects on the database.
