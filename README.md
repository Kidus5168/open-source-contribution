PR Link: https://github.com/beetbox/beets/pull/6812

Summary: Added a missing test case to the convert plugin in beets. The --playlist option generates an m3u8 playlist of converted files, but there was no test verifying that when never_convert_lossy_files=True prevents a lossy file from being transcoded, the playlist keeps the original file extension. I added that test case to test_playlist_entry.

Feedback received:

Bot reminded me to add a changelog entry
Codecov confirmed all tests pass and coverage improved
Status: Awaiting review
