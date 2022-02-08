-- Initial setup --

Copy citus_warp and pgcopydb executables into /usr/local/bin
Create and cd into some folder, e.g. ~/migration
Execute 'citus_warp --setup' to define source/destination server and database
Create a folder to store CDC buffer, output of pg_dump, etc, e.g.: /tmp/warp

-- Migrate --

citus_warp --source-redirect --tmp-dir=/tmp/warp --parallel-dump=8 --parallel-restore=8 --batch-transactions=10 --batch-statements=50 --pgcopydb


Warp options

In order to enable CDC buffer, add option: --cdc-buffer

--parallel-dump=value
    Number of parallel operations whilst dumping the initial sync data (default 1 = not parallel) (default 1)
    In case of pgcopydb this is the number of parallel copy operations.
    
--parallel-restore=value
    Number of parallel operations whilst restoring the initial sync data (default 1 = not parallel) (default 1)
    In case of pgcopydb this is the number of parallel index builds.

-- Cleanup --

citus_warp --clean

Must be run when migration finishes to drop the repliction slot on the source. Also needs to be run before a migration rerun.