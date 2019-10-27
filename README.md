

# TOP 50 THIRD-PARTY LIBRARIES




# APPROACH

<h4>Approach To Accessing Python Files</h4>

The github API was used for grabbing only python files in each repo for analysis. As opposed to cloning each repo locally, this approach cut down on bandwidth and the time to download and go through tons of repo files that were irrelevant to the cause - some repositories can be large.

The github API provides a 'contents' endpoint that returns a json file with information on the folders and files in a repository, and the links to download raw files. More information is available at https://developer.github.com/v3/repos/contents/. 

Using the requests library, python files were read programmatically, and a list of imported libraries obtained using regular expressions. This was compared with the standard library list obtained by scraping https://docs.python.org/3/library/ and
  https://docs.python.org/2.7/library/, to filter third-party libraries. Scraping was performed using the custom script, 'scrape_pystdlib.py'.

The only challenge with the API was the limit on the number of requests in an hour. Without authentication, this was a meagre 60 requests. Authenticated requests were better at a limit of 5000, even though continued requests after the limit was reached, and before a designated reset time, would incur a ban.

<img src="https://github.com/ayivima/python_trends/blob/master/imgs/sample_screen1.png" alt="Sample Program Screen">

These barriers were overcome by a number of strategies including:
- Using authenticated requests
- Script automatically pausing when a check limit of 4900 requests was passed, and resuming after reset time had passed. 

Given the huge workload, it was paramount to use multiple instances running concurrently. Five t2.micro instances were used, and a custom load balancing script was implemented to allocate jobs, monitor progress, and recovery from process termination.

Problematic repositories were skipped.

<img src="https://github.com/ayivima/python_trends/blob/master/imgs/sample_error_screen2.png" alt="Sample Error Screen">
