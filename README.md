# quic-censorship

[A Documentation of Observed QUIC and HTTP/3 Censorship.](document.md)

[Research and Approaches for QUIC Censorship Circumvention Strategies.](evade.md)

[How to Test HTTP/3 in Browsers](browsers.md)

The [measurement list](measurement_list.csv) contains all HTTP/3 measurements taken with OONI's experimental network test `urlgetter`. 
A single report file with HTTP/3 measurements can be downloaded with `aws s3`, using the `URLNAME` from the `url` column in the [measurement list](measurement_list.csv): <br/>
`aws s3 --no-sign-request cp $URLNAME /destination/folder/filename;`

If you want to search for and evaluate HTTP/3 measurements yourself, check out [kelmenhorst/http3-toolchain](https://github.com/kelmenhorst/http3-toolchain) or the easy to use [Colab notebook](https://colab.research.google.com/drive/1d-UWvDsAHLGFwY583J9kckCfBJfdpmh6?usp=sharing).
