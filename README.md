# Contribution #1: Convert Plugin Playlist Uses Original File Extension Instead of Converted Extension

**Contribution Number:** 1
**Student:** Kidus Getachew
**Issue:** https://github.com/beetbox/beets/issues/5786
**Status:** Phase II Complete

## Why I Chose This Issue

I chose this issue because it involves debugging a real-world open-source bug in an established software project. The issue has clear reproduction steps and affects a user-facing feature, making it a good opportunity to practice understanding an existing codebase and contributing a meaningful improvement.

This issue matches my software engineering goals because it involves debugging, understanding program flow, and working with a Python-based application. I am interested in learning how open-source projects structure their code, how bugs are traced, and how fixes are validated through testing.

---

# Understanding the Issue

## Problem Description

The beets `convert` plugin successfully converts audio files into a new format, but when creating an M3U playlist, the playlist references the original file extension instead of the converted file extension.

For example, a `.flac` file is converted into an `.opus` file, but the generated playlist still points to the `.flac` file. This causes the playlist to reference a file that may no longer exist.

## Expected Behavior

When generating a playlist after conversion, the playlist should contain paths to the converted files.

Example:

```
Artist/Album/song.opus
```

## Current Behavior

The conversion creates the new file successfully:

```
song.opus
```

but the playlist contains:

```
song.flac
```

The playlist is pointing to the original source file instead of the converted output.

## Affected Components

* beets convert plugin
* playlist generation
* file path handling
* M3U playlist creation

---

# Reproduction Process

## Environment Setup

OS: macOS
Python Version: 3.9.6
beets Version: 2.3.1 / 2.5.1 (update with your installed version)

## Steps to Reproduce

1. Install beets.
2. Enable the `convert` and `playlist` plugins.
3. Import a test audio file:

```
beet import -A ~/Music/Test
```

4. Confirm the file exists in the beets library:

```
beet list -f '$path'
```

5. Convert the file and generate a playlist:

```
beet convert -m test.m3u
```

6. Check the converted output:

```
find ~/TranscodedMusic -type f
```

7. Check the generated playlist:

```
cat ~/TranscodedMusic/test.m3u
```

## Observed Result

The converted file is created successfully, but the generated playlist references the original `.flac` file instead of the converted `.opus` file.

## Reproduction Evidence

Commit showing reproduction:

To be completed.

Screenshots/logs:

The reproduction output showed that the playlist contained the original extension while the converted directory contained the new converted file.

## My Findings

The issue does not appear to be in the audio conversion process because the converted file is generated correctly. The problem appears during playlist creation, where the wrong file path is being used.

---

# Solution Approach

## Analysis

The likely cause is that playlist generation uses the original library item path instead of the converted destination path created by the convert plugin.

The fix will require locating where playlist entries are created and ensuring the converted path is used.

## Proposed Solution

Update the playlist generation process so that generated playlists reference converted files instead of the original source files.

## Implementation Plan

### Understand

Investigate how the convert plugin stores source and destination paths.

### Match

Review existing path handling patterns in the codebase and identify similar functionality.

### Plan

1. Locate the convert plugin implementation.
2. Find the code responsible for creating playlist entries.
3. Determine where the incorrect file path is selected.
4. Replace the original path reference with the converted path.
5. Add tests to verify playlist output.

### Implement

Branch:

https://github.com/Kidus5168/beets/tree/fix-playlist-extension

### Review

* Follow beets contribution guidelines.
* Ensure code style matches the project.
* Verify tests pass.

### Evaluate

Confirm that generated playlists point to converted files and that existing conversion behavior is unchanged.

---

# Testing Strategy

## Unit Tests

Test cases:

1. Verify playlists contain converted file extensions.
2. Verify converted paths are used instead of source paths.
3. Verify existing playlist functionality still works.

## Integration Tests

1. Convert FLAC files to Opus and generate playlists.
2. Verify generated M3U files contain valid paths.

## Manual Testing

Run:

```
beet convert -m test.m3u
```

Verify:

* Converted files are created.
* Playlist entries match converted files.

---

# Implementation Notes

## Week 2 Progress

* Set up beets environment.
* Reproduced the playlist extension issue.
* Documented expected and current behavior.



# Learnings & Reflections

## Technical Skills Gained

* Debugging an unfamiliar open-source codebase.
* Understanding Python plugin architecture.
* Learning how software projects handle file paths and generated output.

## Challenges Overcome

The main challenge was setting up the environment and reproducing the issue locally. After configuring beets and importing a test file, the issue was successfully reproduced.

## What I'd Do Differently Next Time

I would spend more time understanding the project structure before reproduction and identify likely code locations earlier.

---

# Resources Used

* https://github.com/beetbox/beets/issues/5786
* beets documentation
* GitHub repository documentation
