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


A description of data issues from a researchers perspective is available at [rpki-study.github.io](https://rpki-study.github.io/rpki-archive/)

## Changelog
Dates are the date of the change in the processing. They are likely reflected started in the file that starts on the next day.

#### 2022-04-12:

  * Re-uploaded all archives between 2022-02-18 and 2022-04-05 (including) to correct a 32-byte prefix in files in the unvalidated paths of the archives.

**Resolves:** 32-byte prefix in the historic archives.

#### 2022-04-06:

  * Routinator updated from 0.10.1 to 0.11.1-rc1

**Resolves:** 32-byte prefix on files in the unvalidated paths of the archives from this day on.

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

#### < 2021-10-01:

  * rpki-validator 2 was used
  * The roa.csv file is missing from a large number of repos.
