# Contribution #1: Convert Plugin Playlist Uses Original File Extension Instead of Converted Extension

## Contribution Number
1

## Student
Kidus Getachew

## Issue
https://github.com/beetbox/beets/issues/5786

## Status
Phase III In Progress


# Why I Chose This Issue

I chose this issue because it involves debugging a real-world open-source bug in an established software project. The issue has clear reproduction steps and affects a user-facing feature, making it a good opportunity to practice understanding an existing codebase and contributing a meaningful improvement.

This issue matches my software engineering goals because it involves debugging, understanding program flow, and working with a Python-based application. I am interested in learning how open-source projects structure their code, how bugs are traced, and how fixes are validated through testing.


# Understanding the Issue

## Problem Description

The beets `convert` plugin successfully converts audio files into a new format, but when creating an M3U playlist, the playlist references the original file extension instead of the converted file extension.

For example, a `.flac` file is converted into an `.opus` file, but the generated playlist still points to the `.flac` file. This causes the playlist to reference a file that may no longer exist.


## Expected Behavior

The generated playlist should reference the converted file.

Example:


Artist/Album/song.opus



## Current Behavior

The converted file is created successfully:


song.opus


However, the generated playlist contains:


song.flac


The playlist points to the original source file instead of the converted output.


## Affected Components

- Convert plugin
- Playlist generation
- File path handling
- M3U playlist creation


# Reproduction Process

## Environment Setup

OS: macOS

Python Version: 3.9.6

beets Version: 2.5.1


## Steps to Reproduce

1. Install beets.

2. Enable the convert and playlist plugins.

3. Import a test audio file:

```bash
beet import -A ~/Music/Test
Confirm the imported file:
beet list -f '$path'
Convert the file and generate a playlist:
beet convert -m test.m3u
Check converted output:
find ~/TranscodedMusic -type f
Check generated playlist:
cat ~/TranscodedMusic/test.m3u
Observed Result

The converted file is generated correctly, but the playlist references the original .flac file instead of the converted .opus file.

Reproduction Evidence

Commit showing reproduction:

To be added.

Screenshots/logs:

The generated playlist output showed that the playlist contained the original extension instead of the converted file extension.

My Findings

The conversion process itself works correctly. The issue appears during playlist generation where the original source path is used instead of the converted destination path.

Solution Approach
Analysis

The likely cause is that playlist generation is using the original library item path instead of the converted file path created by the convert plugin.

Proposed Solution

Update the playlist generation logic so that playlist entries reference converted files instead of the original source files.

Implementation Plan
Understand

Investigate how the convert plugin stores source and converted file paths.

Match

Review existing path handling patterns in the beets codebase.

Plan
Locate the convert plugin implementation.
Find where playlist entries are created.
Identify where the incorrect file path is selected.
Replace the original path reference with the converted path.
Add tests to verify playlist output.
Implement

In progress.

Review
Follow beets contribution guidelines.
Ensure code style matches the project.
Verify tests pass.
Evaluate

Confirm generated playlists point to converted files and existing functionality is unchanged.

Testing Strategy
Unit Tests

Planned:

Verify playlists contain converted file extensions.
Verify converted paths are used instead of source paths.
Verify existing playlist functionality remains unchanged.
Integration Tests

Planned:

Convert FLAC files to Opus and generate playlists.
Verify generated M3U files contain valid paths.
Manual Testing

Completed:

Reproduced the original issue locally.

Planned:

Verify the fix after implementation.
Implementation Notes
Phase III Progress
What I built:
Started Phase III investigation for issue #5786.
Set up development workflow using my fork.
Reproduced the reported bug locally.
Confirmed the issue occurs during playlist generation after conversion.
Challenges Faced:
Understanding an unfamiliar open-source codebase.
Identifying where converted file paths are handled.
Next Steps:
Locate the playlist generation logic.
Implement the fix.
Add regression tests.
Submit a pull request.
Code Changes
Branch

](https://github.com/Kidus5168/beets/tree/fix-playlist-extension)
Files Modified

None yet.

Commits

To be added after implementation.

Pull Request
PR Link

To be added.

PR Description

To be added.

Maintainer Feedback

To be added.

Learnings & Reflections
Technical Skills Gained
Debugging an unfamiliar open-source project.
Understanding Python plugin architecture.
Learning how file paths are handled during conversion workflows.
Challenges Overcome

The main challenge was setting up the environment and reproducing the issue. After configuring beets and importing a test file, the bug was successfully reproduced.

What I'd Do Differently Next Time

I would explore the project structure earlier and identify relevant files before beginning implementation.

Resources Used
https://github.com/beetbox/beets/issues/5786
beets documentation
GitHub repository documentation
