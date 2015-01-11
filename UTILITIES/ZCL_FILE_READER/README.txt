ZCL_FILE_READER

This reads any file (at the moment CSV, XLS and XLSX are implemented)
and imports the data into a table, converting from external to internal

*********************************************************

Installation notes

All files are installed using SAPLINK.

1 - Install file NUGG_FILE_READER_PART1. This contains the classes and interfaces
necessary. Navigate to the package you installed to using SE80 (or equivalent)
and activate everything. You will get an error, choose Activate anyway.

2 - Install file NUGG_FILE_READER_PART2. This contains the enhancement spot
used for the BAdIs. Navigate to the package and activate everything.

3 - Install file NUGG_FILE_READER_PART3. This contains the BAdI implementations.
Navigate to the package and activate everything.

4 (optional) - Install file NUGG_FILE_READER_TEST_REP. This contains a DEMO
report to exemplify how the file reading class works. Use any of the demo
files, sit back, relax and enjoy the show.