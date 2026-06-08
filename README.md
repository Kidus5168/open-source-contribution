# open-source-contribution

Contribution [#]: Convert Plugin Playlist Uses Original File Extension Instead of Converted Extension

Contribution Number: 1

Student: Kidus Getachew

Issue: https://github.com/beetbox/beets/issues/5786

Status: Phase I - Complete

Why I Chose This Issue

I chose this issue because it involves debugging a real-world software defect in a mature open-source project. The issue appears to be well-defined, includes reproduction steps, expected behavior, and configuration details, making it an approachable first contribution while still requiring investigation of an unfamiliar codebase.

This issue also aligns with my interests in software engineering and backend development. Solving it will require understanding how the beets conversion pipeline generates playlist files and how file paths are updated after transcoding. I hope to gain experience navigating a large Python codebase, tracing program execution, and contributing a fix that directly improves user functionality.

Understanding the Issue
Problem Description

The convert plugin generates an M3U playlist after transcoding audio files. However, the generated playlist references the original source file extension rather than the newly converted file extension.

For example, a FLAC file is successfully converted into an Opus file, but the playlist still points to the original .flac file. As a result, the generated playlist contains invalid file references and cannot correctly play the transcoded files.

Expected Behavior

When the convert command creates a playlist, each playlist entry should reference the converted file that was actually generated.

Example:

RichaadEB/Lifelight (English Version)/01 Lifelight - English Version.opus

Current Behavior

The audio file is successfully converted to Opus format:

01 Lifelight - English Version.opus

However, the generated playlist references:

01 Lifelight - English Version.flac

which no longer matches the converted output.

Affected Components
Convert plugin
Playlist generation logic
Path construction for converted files
M3U playlist creation functionality
Reproduction Process
Environment Setup

The issue was reported on Linux (NixOS 25.05) using Python 3.12.10 and beets 2.3.1. The reporter provided a complete configuration and reproduction example. Environment setup and local reproduction will be completed during Phase II.

Steps to Reproduce
Configure beets with the convert plugin enabled.
Set output format to Opus.
Run:

beet convert -m test.m3u lifelight

Verify the converted output file is generated.
Open the generated playlist.

Observed Result:

The playlist references the original FLAC file instead of the converted Opus file.

Reproduction Evidence

Commit showing reproduction: To be completed in Phase II.

Screenshots/logs: Included in issue report.

My findings:

The conversion process itself completes successfully. The defect appears to occur when playlist entries are generated after conversion.

Solution Approach
Analysis

My initial hypothesis is that the playlist generator is using the original source file path metadata instead of the converted destination path returned by the conversion process.

Because the converted file is correctly created, the bug likely exists in playlist generation rather than audio conversion.

Proposed Solution

Investigate where playlist entries are created after conversion and ensure the converted destination path is used when writing playlist entries.

Implementation Plan
Understand

Determine how the convert plugin tracks source and destination file paths during transcoding.

Match

Identify similar code paths that generate file references after conversion or relocation operations.

Plan
Locate convert plugin implementation.
Trace playlist generation workflow.
Identify where file paths are selected for playlist output.
Replace source-path usage with converted-path usage if appropriate.
Add or update automated tests.
Verify generated playlists reference converted files.
Implement

To be completed during Phase III.

Review
Follow project contribution guidelines.
Verify style and formatting requirements.
Confirm tests pass.
Evaluate

Verify that generated playlists reference converted output files and that existing functionality remains unchanged.

Testing Strategy
Unit Tests
Verify playlist entry uses converted extension.
Verify playlist entry uses converted destination path.
Verify existing conversion functionality remains intact.
Integration Tests
Convert FLAC to Opus and generate playlist.
Convert multiple files and verify all playlist entries.
Manual Testing

To be completed during implementation.

Learnings & Reflections

This issue demonstrates how a small path-handling bug can affect user-facing functionality even when the primary conversion process succeeds. I expect to learn more about Python plugin architectures, debugging workflows, and contributing fixes to established open-source projects.

Resources Used
GitHub issue #5786
beets project documentation
beets contributor documentation
