# Sable Media FM screens: approved proposal

The owner's approved proposal for the Media app's FM and related screens on the Q25 (720x720),
as six screens: [`artifacts/sable-media-fm-screens-proposal.pdf`](artifacts/sable-media-fm-screens-proposal.pdf).

1. **FM dial:** big frequency with MHz under it, a dial that scrolls round the tuned station, then
   `Live FM · <call sign> · <genre>`, the station name in bold with a star, and the song with its picture.
   Bottom bar: Previous, Pause, Next, Stations, Record. Hint line: `Space on/off · J/K station ·
   Shift+J/K 0.1 · Alt+1-9 presets · F star · R record · O output`.
2. **Stations:** tabs All, Favourites, Presets; one row per station with frequency, name, call sign and
   genre, a star on favourites; the focused row is filled with the accent. Bar: Previous, Pause, Next, Dial, Favourites.
3. **Favourites:** songs, FM stations, online radio and playlists in one list with their length or LIVE.
4. **Lists:** playlists with song and station counts and total time.
5. **FM recording:** a red REC timer under MHz; the bar shows Stop rec and Output.
6. **Recordings:** FM and music recordings with date, call sign and length.

Status of the build against it is tracked in the sableos-android Media app (`apps/android/media`).
