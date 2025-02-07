# Release Date Cleaning Function
## Background
The release dates Luminate provides for songs are typically accurate ~50% of the time. These errors can be caused by several different factors. For example, if a music video releases before the official audio, Luminate will log the MV release date as the song's release date. In other scenarios, release dates might be off by 1 day because of how they ingest metadata internally. Regardless, in order to obtain accurate first week numbers, we need to identify the most likely release date.
## Goal: Identify most likely release date for each song
**Required Columns**: song_id, date, streams

**How**: Look at the day-over-day streams differentials for the first 21 days from the first recorded non-zero streaming day. The day with the greatest day-to-day difference is the most likely release date for that song.

**Steps**:

1. Create a DataFrame of daily streaming data for one or more songs
2. Clean df data types to ensure streams are integers, dates are dates, artists are strings, etc.
3. Copy df
4. Create 'streams diff' column showing the day-to-day streaming differences
5. Establish the first non-zero streaming date and create a 21 day window from there
6. Return a 'release_dates' df with 1 row per song_id corresponding to the max 'streams diff' value. We will consider the corresponding date in each row as that song's most likely release date
7. Create new DataFrame clean_df (we will load all songs starting from their most likely release date in here)
8. Iterate through 'release_dates' df and collect all song data from the original df where the song_ids match while recorded the release date as a variable
9. Load the song's streaming data starting from the most likely release into clean_df
10. Return clean_df

## Result
All streaming data from the songs in the original DataFrame starting from the most likely release date
