# Obsidian BioLabNote Template

An Obsidian template set for running a lab notebook with linked daily entries, experiment notes, and monthly milestones.

This repository contains the template notes and Templater scripts I use to keep experiment records structured, searchable, and connected over time.

## What is included

- `Experiment template 2026.md` for individual experiment records
- `Milestone template.md` for monthly or project milestones
- `Scripts/Daily Entry Script.md` to generate or reopen the current day's lab note
- `Scripts/Experiment Script.md` to create experiments with automatic date-based codes
- `Scripts/Milestone Script.md` to create a dated milestone note

## Recommended Obsidian plugins

- Templater
- Dataview

The templates are written for a vault that uses both plugins. Without them, the automation and tables will not work as intended.

## Expected folder structure

The scripts assume the following folders exist in your vault:

```text
LabNote/
  Daily Entries/
  Experiments/
  MileStones/
```

If your vault uses different paths, update the folder values inside the script files before use.

## How the workflow works

### Daily entry

Run `Scripts/Daily Entry Script.md` from Templater.

The script:

- opens today's daily note if it already exists
- otherwise creates a note named `YYYY-MM-DD`
- stores it in `LabNote/Daily Entries`
- lists notes tagged `#OnGoingExperiments`
- creates a section stub for each ongoing experiment

### Experiment note

Run `Scripts/Experiment Script.md`.

The script:

- generates a code in the format `EYYMMDDA`, `EYYMMDDB`, and so on
- prompts for a file name
- keeps the generated code as the first token in the note title
- creates the note in `LabNote/Experiments`
- writes the experiment code into frontmatter

To have an experiment appear in the daily entry table, include `#OnGoingExperiments` in that experiment note.

### Milestone note

Run `Scripts/Milestone Script.md`.

The script creates a note named like `Mar 2026` and moves it to `LabNote/MileStones`.

## Installation

1. Download or clone this repository.
2. Copy the template files and the `Scripts` folder into your Obsidian vault.
3. Create the required `LabNote` folders if they do not already exist.
4. In Obsidian, enable `Templater` and `Dataview`.
5. Point Templater to the folder where you placed these templates.
6. Run the scripts through Templater commands, hotkeys, or templates as preferred.

## Notes

- The daily note script references `![[Scheduler#TODO]]`. Remove or adapt that line if your vault does not contain a `Scheduler` note.
- The current milestone folder name is `MileStones` because that is what the script expects. You can rename it, but then update the path inside `Scripts/Milestone Script.md`.

## Repository purpose

This repository is meant to share the template set itself, not a full Obsidian vault. Keeping the repo small makes it easier to reuse in different projects and lab notebooks.
