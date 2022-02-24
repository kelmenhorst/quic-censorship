# How to test HTTP/3 in browsers
Firefox and Chrome/Chromium browsers have added HTTP/3 support by default in 2020/2021. In order to discover that a given server runs HTTP/3, the browsers usually start with a "normal" HTTPS (TCP) request first: If the ```Alt-Svc``` (alternative service) header of the HTTP reply contains ```h3```, the browser proceeds by requesting the website over HTTP/3. **Important: This mechanism will not work if your network censors the first HTTPS (TCP) request.** However, there are some (not so practical) ways to force your browser to just use HTTP/3, without having to run HTTPS (TCP) before:

## Firefox
Firefox has HTTP/3 support enabled by default since version 88 (April 2021).
If you want to force using HTTP/3 for certain hosts, there is a configuration option for testing that you can modify:
1. Type ```about:config``` in the URL bar.
2. Confirm ```Accept the Risk and Continue``` 
3. Type ```network.http.http3.alt-svc-mapping-for-testing``` in the search bar.
4. Select the Edit button on the right. 
5. For each host that you want to force Firefox to use HTTP/3 on, add an entry: ```<host-name>;h3=":443";h3-29=":443"```. Entries are separated by commas.
    - Example: <br/> ```www.greenpeace.org;h3=":443";h3-29=":443",www.youtube.com;h3=":443";h3-29=":443"```
6. Save, close the config page and restart Firefox.
7. Type the address of the website in the URL bar (use the exact domain name specified in the configuration entry).

## Chromium (Google Chrome)
Chrome has HTTP/3 support enabled by default since version 87 (April 2020).

You can force using HTTP/3 for a single host by starting Chromium/Chrome from the command line.
1. Type the follwing command: <br/>
Chromium: ```chromium --enable-quic --origin-to-force-quic-on=www.greenpeace.org:443``` <br/>
Chrome: ```chrome --enable-quic --origin-to-force-quic-on=www.greenpeace.org:443```
2. Type the address of the website in the URL bar (use the exact domain name specified in the command).
