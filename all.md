## Second cloud assignment


   
diskusage.sh

#!/bin/bash

## Default value for the number of entries
NUM_ENTRIES=8

## Parse command line arguments
while getopts ":dn:" opt; do
  case ${opt} in
    d )
      LIST_FILES=true
      ;;
    n )
      NUM_ENTRIES=$OPTARG
      ;;
    \? )
      echo "Usage: $0 [-d] [-n N] directory" 1>&2
      exit 1
      ;;
    : )
      echo "Invalid option: $OPTARG requires an argument" 1>&2
      exit 1
      ;;
  esac
done
shift $((OPTIND -1))

## Get the directory path
DIR=$1

## Check if directory is provided
if [ -z "$DIR" ]; then
  echo "Directory argument is missing!" >&2
  exit 1
fi

## Check if directory exists
if [ ! -d "$DIR" ]; then
  echo "Directory '$DIR' does not exist!" >&2
  exit 1
fi

## Get disk usage and sort by size
if [ "$LIST_FILES" = true ]; then
  du -ah "$DIR" 2>/dev/null | sort -rh | head -n "$NUM_ENTRIES"
else
  du -h "$DIR" 2>/dev/null | sort -rh | head -n "$NUM_ENTRIES"
fi




backupscript.sh


#!/bin/bash

## Check if two arguments are provided
if [ "$#" -ne 2 ]; then
  echo "Usage: $0 source_directory destination_directory" >&2
  exit 1
fi

# Check if source directory exists
if [ ! -d "$1" ]; then
  echo "Source directory '$1' does not exist!" >&2
  exit 1
fi

## Create destination directory if it does not exist
mkdir -p "$2"

## Create backup with timestamp
TIMESTAMP=$(date '+%Y%m%d%H%M%S')
tar -czf "$2/backup_$TIMESTAMP.tar.gz" "$1"

echo "Backup created successfully at '$2/backup_$TIMESTAMP.tar.gz'"





Make sure to give execute permissions to both scripts using chmod +x disk_usage_checker.sh backup_script.sh.

## To use the disk usage checker script:


./disk_usage_checker.sh -n 5 /var
## To list both directories and files:


./disk_usage_checker.sh -d /var
For the backup script:


./backup_script.sh source_directory destination_directory
    Replace source_directory and destination_directory with actual directory paths.









    