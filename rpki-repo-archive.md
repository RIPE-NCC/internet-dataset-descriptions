# RPKI repo archive

The RPKI repo archive is at https://ftp.ripe.net/rpki/ 

The archive is structured as follows:
   https://ftp.ripe.net/rpki/TAL/YYYY/MM/DD/
with:
   * `TAL` : Trust anchor [1]
   * `YYYY` : Year
   * `MM`   : Month
   * `DD`   : Day

The individual daily directories per trust anchor contain 2 files:
   * `repo.tar.gz`: The raw repository content (as a tar-gzipped archive)
   * `roas.csv`: The VRPs (Verified ROA Payloads) that were extacted from the PKI materials

##  Data Issues

## 32-byte prefix in unvalidated files

Files in the unvalidated path of the archives have a 32-byte prefix. This exists in archives from 2022-02-18 to 2022-04-05 (including).

## Incomplete APNIC data

There was an issue with syncing the APNIC data from ±2021-10-01 to 2022-02-23 , which results in roughly half of the days having truncated data

Around October 2021 the data pipeline changed. Before this day rpki-validator-2 was used, after that day routinator 0.10.1 used.

The roa.csv file is missing from a large number of repos.

## Changelog
Dates are the date of the change in the processing. They are likely reflected started in the file that starts on the next day.

#### 2022-04-06:

  * Routinator updated from 0.10.1 to 0.11.1-rc1

**Resolves:** 32-byte prefix on files in the unvalidated paths of the archives.

#### 2022-02-23:
  * `routinator.log` now contains errors + verbose output.
    * steps we execute changed: `routinator update`, `routinator vrps --no-update`, `routinator dump`
  * Trust Anchor certificate added to the archive (directly [for now](https://github.com/NLnetLabs/routinator/issues/722))

**Resolves:** trust anchor certificates are included in the dataset.  
**Resolves:** large fraction of days with partial data for APNIC

#### 2022-02-18:
  * `rrdp` is enabled. This should resolve the updates containing only partial data for APNIC.
  * `routinator.log` containing errors in routinator output was added.

**Artifact:** change in directory structure of output (RRDP hostnames are present in `repo.tar.gz` archive).  
**Known issue:** trust anchor certificates are not present in output (and may have been for a while)

#### 2022-02-15:
The containers running the data collection job have IPv6 connectivity

#### 2021-10-01 (approximate):

Data collection switched from rpki-validator-2 to routinator 0.10.1.

  * routinator starts with a clean cache every day.
  * `rrdp` is not enabled (similar to rpki-validator-2).
  * The container running the job does not have IPv6 connectivity.

**Known issue:** A large fraction of the days has partial data for APNIC.

Another description of this dataset is at https://rpki-study.github.io/rpki-archive/
TODO document this better.
