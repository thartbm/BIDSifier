# BIDSifier

This should have one usable function: `BIDSify()`, that can be pointed at the `data` folder of the Intrepid 2a project and convert the behavioral files to BIDS format. The function will create or overwrite a folder called `Intrepid2a_BIDS` in the `data` folder. This code is built with the format used at the replication lab in Toronto in mind, so it may or may not work in Glasgow.

## BIDS

Brain Imaging Data Structure is essentially a very strict folder hierarchy and naming scheme, where all data is either in json (metadata) or tsv or csv (behavioral data) files. There may also be binary data files (in our case with eye-tracking data), but we will not have those for now. The metadata files (.json format) seem to always have the same name as the file they explain.

Within the BIDS folder for a project, there is at least 1 files, and potentially a lot of sub directories. We need this file:

- `participants.csv`, and possibly:
- `participants.json` to explain the contents of the other files

There may also be json files explaining the content of the files found in the subdirectories.

It seems that usually all sub-directory covers all data from a single participant. These are to be named `sub-<identifier>` where <identifier> is the unique identifier of that participant. There can be other sub-directories with optional data. We could for example generate `derivate` files that have all behavioral responses from all participants on each of the tasks.

It seems that within each participant's subdirectory, there is another directory for each type of data. In our case that is all behavioral, and should be in a folder called `beh`.

BIDS uses the concept of a "session" but I don't think we need this, since task already describes this sufficiently.

BIDS also uses "runs" which we can use for the left and right hemifield runs, or perhaps we should just combine those into 1 run? Either way, runs can be indexed, or labeled, and labeled would make more sense in our case. (left hemifield runs as 'LH' and right hemifield runs as 'RH').

File extensions include the left most period: `.csv` for example.

There is a pretty abstract concept of an "entity" which could be a participant, a task or a run. A so-called "entity" is to be named using a "<key>-<value>" pair in file names. For example, "sub-01" for participant 1, but we could also use "run-LH" and "run-RH".

We may also have `task` as an entity, and this would give these 3 parts of file names: "task-area", "task-curvature" and "task-distance" perhaps the last two should be abbreviated to "task-curv" and "task-dist".

Entities are separated by underscores. This would mean filenames for the behavioral task data would be:

- "sub-<ID>_task-<task>_run-<hemifield>.csv"

I'm not sure how the two calibration files should get in there: color calibration and blind spot mapping. Perhaps they are not runs, but calibrations: "calib-color" and "calib-bsmap". A decision on this can wait for a bit, but file names could look like:

- "sub-<ID>_task-<task>_calib-<type>.csv"

This means that for each task, there would be 4 files: 1) color calibration, 2) blind spot mapping and the behavioral responses for 3) the left hemifield and 4) the right hemifield.

