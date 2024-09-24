# Database Code By Release Directories

This repository demonstrates how to organize database scripts in directories organized by releases.

## Directory Structure
Here is the high-level overview of the directory structure. Note that there is a directory for each release (`1.0`, `1.1`, `2.0` and so on). 
```
.
├── README.md
├── liquibase.properties
└── sqlcode
    ├── 1.0
    ├── 1.1
    ├── 2.0
    ├── 2.1
    ├── 2.2
    ├── 3.0
    └── rootchangelog.yaml
```

Here is the entire directory structure of this repo. There are database subdirectories in each release directory. And each database subdirectory has its own changelog file (e.g., `database1changelog.yaml`). 
```
.
├── README.md
├── liquibase.properties
└── sqlcode
    ├── 1.0
    │   ├── changelog.yaml
    │   ├── database1
    │   │   └── changelog.yaml
    │   ├── database2
    │   │   └── changelog.yaml
    │   └── database3
    │       └── changelog.yaml
    ├── 1.1
    │   ├── changelog.yaml
    │   ├── database1
    │   │   └── changelog.yaml
    │   ├── database2
    │   │   └── changelog.yaml
    │   └── database3
    │       └── changelog.yaml
    ├── 2.0
    │   ├── changelog.yaml
    │   ├── database1
    │   │   └── changelog.yaml
    │   ├── database2
    │   │   └── changelog.yaml
    │   └── database3
    │       └── changelog.yaml
    ├── 2.1
    │   ├── changelog.yaml
    │   ├── database1
    │   │   └── changelog.yaml
    │   ├── database2
    │   │   └── changelog.yaml
    │   └── database3
    │       └── changelog.yaml
    ├── 2.2
    │   ├── changelog.yaml
    │   ├── database1
    │   │   └── changelog.yaml
    │   ├── database2
    │   │   └── changelog.yaml
    │   └── database3
    │       └── changelog.yaml
    ├── 3.0
    │   ├── changelog.yaml
    │   ├── database1
    │   │   └── changelog.yaml
    │   ├── database2
    │   │   └── changelog.yaml
    │   └── database3
    │       └── changelog.yaml
    └── rootchangelog.yaml
```



## Changelog files
Note that there is a [rootchangelog.yaml](sqlcode/rootchangelog.yaml) which serves as the entry point changelog for Liquibase (as specified in [liquibase.properties](liquibase.properties) file). This file points to `changelog.yaml` file in each release directory:
```yaml
databaseChangeLog:

- include:
    file: 1.0/changelog.yaml
    relativeToChangelogFile: true

- include:
    file: 1.1/changelog.yaml
    relativeToChangelogFile: true

- include:
    file: 2.0/changelog.yaml
    relativeToChangelogFile: true

- include:
    file: 2.1/changelog.yaml
    relativeToChangelogFile: true

- include:
    file: 2.2/changelog.yaml
    relativeToChangelogFile: true

- include:
    file: 3.0/changelog.yaml
    relativeToChangelogFile: true

```

`changelog.yaml` file in each release directory points to a changelog.yaml file in each database directory:
```yaml
databaseChangeLog:

- include:
    file: database1/changelog.yaml
    relativeToChangelogFile: true

- include:
    file: database2/changelog.yaml
    relativeToChangelogFile: true

- include:
    file: database3/changelog.yaml
    relativeToChangelogFile: true
```

`changelog.yaml` file in each release/databaseX directory is configured with changesets using [sqlFile](https://docs.liquibase.com/change-types/sql-file.html) change type. You would provide `script1`, `script2` or `script3` in each changeset:
```yaml
- changeSet:
    id: script1
    author: nvoxland
    changes:
    - sqlFile:
        path: script1.sql
        relativeToChangelogFile: true
    - rollback:
        - sqlFile:
            path: script1-rollback.sql
            relativeToChangelogFile: true

- changeSet:
    id: script2
    author: nvoxland
    changes:
    - sqlFile:
        path: script2.sql
        relativeToChangelogFile: true
    - rollback:
        - sqlFile:
            path: script2-rollback.sql
            relativeToChangelogFile: true

- changeSet:
    id: script2
    author: nvoxland
    changes:
    - sqlFile:
        path: script2.sql
        relativeToChangelogFile: true
    - rollback:
        - sqlFile:
            path: script2-rollback.sql
            relativeToChangelogFile: true
```