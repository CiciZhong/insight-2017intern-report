# insight-2017intern-report
the report of the NASA traffic insight 2017
# Table of Contents
1. [Challenge Summary](README.md#challenge-summary)
2. [Details of Implementation](README.md#details-of-implementation)
3. [Download Data](README.md#download-data)
4. [Description of Data](README.md#description-of-data)
5. [Writing clean, scalable, and well-tested code](README.md#writing-clean-scalable-and-well-tested-code)
6. [Repo directory structure](README.md#repo-directory-structure)
7. [Testing your directory structure and output format](README.md#testing-your-directory-structure-and-output-format)
8. [Jinghan Zhong report](README.md#faq)


# Challenge Summary

Picture yourself as a backend engineer for a NASA fan website that generates a large amount of Internet traffic data. Your challenge is to perform basic analytics on the server log file, provide useful metrics, and implement basic security measures. 

The desired features are described below: 

### Feature 1: 
List the top 10 most active host/IP addresses that have accessed the site.

### Feature 2: 
Identify the 10 resources that consume the most bandwidth on the site

### Feature 3:
List the top 10 busiest (or most frequently visited) 60-minute periods 

### Feature 4: 
Detect patterns of three failed login attempts from the same IP address over 20 seconds so that all further attempts to the site can be blocked for 5 minutes. Log those possible security breaches.


### Other considerations and optional features
It's critical that these features don't take too long to run. For example, if it took too long to detect three failed login attempts, further traffic from the same IP address couldn’t be blocked immediately, and that would present a security breach.
This dataset is inspired by real NASA web traffic, which is very similar to server logs from e-commerce and other sites. Monitoring web traffic and providing these analytics is a real business need, but it’s not the only thing you can do with the data. Feel free to implement additional features that you think might be useful.

## Details of Implementation
With this coding challenge, you should demonstrate a strong understanding of computer science fundamentals. We won't be wowed by your knowledge of various available software libraries, but will be impressed by your ability to pick and use the best data structures and algorithms for the job.

We're looking for clean, well-thought-out code that correctly implements the desired features in an optimized way and highlights your ability to write production-quality code.

We also want to see how you use your programming skills to solve business problems. At a minimum, you should implement the four required features, but feel free to expand upon this challenge or add other features you think would prevent fraud and further business goals. Be sure to document these add-ons so we know to look for them.

### Feature 1 
List in descending order the top 10 most active hosts/IP addresses that have accessed the site.

Write to a file, named `hosts.txt`, the 10 most active hosts/IP addresses in descending order and how many times they have accessed any part of the site. There should be at most 10 lines in the file, and each line should include the host (or IP address) followed by a comma and then the number of times it accessed the site. 

e.g., `hosts.txt`:

    example.host.com,1000000
    another.example.net,800000
    31.41.59.26,600000
    …


### Feature 2 
Identify the top 10 resources on the site that consume the most bandwidth. Bandwidth consumption can be extrapolated from bytes sent over the network and the frequency by which they were accessed.

These most bandwidth-intensive resources, sorted in descending order and separated by a new line, should be written to a file called `resources.txt`


e.g., `resources.txt`:
    
    /images/USA-logosmall.gif
    /shuttle/resources/orbiters/discovery.html
    /shuttle/countdown/count.html
    …


### Feature 3 
List in descending order the site’s 10 busiest (i.e. most frequently visited) 60-minute period.

Write to a file named `hours.txt`, the start of each 60-minute window followed by the number of times the site was accessed during that time period. The file should contain at most 10 lines with each line containing the start of each 60-minute window, followed by a comma and then the number of times the site was accessed during those 60 minutes. The 10 lines should be listed in descending order with the busiest 60-minute window shown first. 

e.g., `hours.txt`:

    01/Jul/1995:00:00:01 -0400,100
    02/Jul/1995:13:00:00 -0400,22
    05/Jul/1995:09:05:02 -0400,10
    01/Jul/1995:12:30:05 -0400,8
    …

A 60-minute window can be any 60 minute long time period, windows don't have to start at a time when an event occurs.

### Feature 4 
Your final task is to detect patterns of three consecutive failed login attempts over 20 seconds in order to block all further attempts to reach the site from the same IP address for the next 5 minutes. Each attempt that would have been blocked should be written to a log file named `blocked.txt`.

The site’s fictional owners don’t expect you to write the actual web server code to block the attempt, but rather want to gauge how much of a problem these potential security breaches represent. 

Detect three failed login attempts from the same IP address over a consecutive 20 seconds, and then write to the `blocked.txt` file any subsequent attempts to reach the site from the same IP address over the next 5 minutes. 

For example, if the third consecutive failed login attempt within a 20 second window occurred on `01/Aug/1995:00:00:08`, all access to the website for that IP address would be blocked for the next 5 minutes. Even if the same IP host attempted a login -- successful or not -- one minute later at `01/Aug/1995:00:01:08`, that attempt should be ignored and logged to the `blocked.txt` file. Access to the site from that IP address would be allowed to resume at `01/Aug/1995:00:05:09`.

If an IP address has not reached three failed login attempts during the 20 second window, a login attempt that succeeds during that time period should reset the failed login counter and 20-second clock. 

For example, if after two failed login attempts, a third login attempt is successful, full access should be allowed to resume immediately afterward. The next failed login attempt would be counted as 1, and the 20-second timer would begin there. In other words, this feature should only be triggered if an IP has  3 failed logins in a row, within a 20-second window.

e.g., `blocked.txt`

    uplherc.upl.com - - [01/Aug/1995:00:00:07 -0400] "GET / HTTP/1.0" 304 0
    uplherc.upl.com - - [01/Aug/1995:00:00:08 -0400] "GET /images/ksclogo-medium.gif HTTP/1.0" 304 0
    …

The following illustration may help you understand how this feature might work, and when three failed login attempts would trigger 5 minutes of blocking:


![Feature 4 illustration](images/feature4.png)


The directory structure for the report:

    ├── README.md 
    ├── run.sh
    ├── src
    │   └── process_log.py
    ├── log_input
    │   └── log.txt
    ├── log_output
    |   └── hosts.txt
    |   └── hours.txt
    |   └── resources.txt
    |   └── blocked.txt
    ├── insight_testsuite
        └── run_tests.sh
        └── tests
            └── test_features
            |   ├── log_input
            |   │   └── log.txt
            |   |__ log_output
            |   │   └── hosts.txt
            |   │   └── hours.txt
            |   │   └── resources.txt
            |   │   └── blocked.txt
            ├── your-own-test
                ├── log_input
                │   └── your-own-log.txt
                |__ log_output
                    └── hosts.txt
                    └── hours.txt
                    └── resources.txt
                    └── blocked.txt



