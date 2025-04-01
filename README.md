# Introduction to memfile
`memfile` is a super light-weight, os-independent / portable library implementing a small subset of FILE-like operations,
along with other functions, for stream-like write and direct read from memory

# Example
The primary initial use case this targets is for converting data into JSON in small bits at a time
using the https://github.com/liquidaty/json_writer library, e.g.:

```
  memfile_t memfile = memfile_open(1024);
  jsonwriter_handle jsw = jsonwriter_new_stream(memfile_write, memfile);
  ...
  for(...) {
    memfile_reset(memfile);   // reset any previously written data

    jsonwriter_xxx(jsw); ...  // write some JSON

    jsonwriter_flush(jsw);    // flush to memfile

    // do something with my bit of stringified JSON
    void *value_to_print = memfile_data(memfile);
    off_t length_to_print = memfile_tell(memfile);
    ...
  }
  ...
  memfile_close(memfile);
  jsonwriter_delete(jsw);
```

# API
See [include/memfile.h](include/memfile.h)
