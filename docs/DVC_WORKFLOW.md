# DVC Workflow

## Remote Configuration

A local folder was used as the DVC remote storage:

~/dvc-remote-storage

The remote was configured using:

python -m dvc remote add -d myremote ~/dvc-remote-storage

## Dataset Versioning Workflow

The following workflow was used for every dataset change:

1. dvc add data/raw/iris_v1.csv
2. git add data/raw/iris_v1.csv.dvc
3. git commit
4. python -m dvc push

Git tracks the `.dvc` metadata file, while DVC stores the actual dataset in its cache and remote storage.

## Dataset Versions

- Version 1: 150 rows
- Version 2: 170 rows

## Comparing Versions

The dataset versions were compared using:

python -m dvc diff HEAD~1

The result showed:

Modified:
data/raw/iris_v1.csv

## Restoring Historical Versions

Version 1 was restored by checking out its `.dvc` file and running:

git checkout 32de032 -- data/raw/iris_v1.csv.dvc

python -m dvc checkout data/raw/iris_v1.csv.dvc

The result was 151 lines, meaning 150 data rows + 1 header.

The latest Version 2 was restored using:

git checkout HEAD -- data/raw/iris_v1.csv.dvc

python -m dvc checkout data/raw/iris_v1.csv.dvc

The result was 171 lines, meaning 170 data rows + 1 header.

## Conclusion

DVC successfully tracked, versioned, compared, pushed, and restored different versions of the dataset while Git tracked the corresponding DVC metadata.