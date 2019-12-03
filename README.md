# GitHub-Api
You can `wget` your way out of the GitHub repo to get a tar file ([archive][1]):

    wget --no-check-certificate https://github.com/User/repo/archive/master.tar.gz

    # better, if the certificate authorities are present:
    wget https://github.com/User/repo/archive/master.tar.gz

will get you a file named 'master' from the user 'User''s repo 'repo'.  

The [updated V3 API url][2] is:

    https://api.github.com/repos/User/repo/:archive_format/:ref
    #
    # two possibilities for fomat:
    https://api.github.com/repos/User/repo/tarball/master
    https://api.github.com/repos/User/repo/zipball/master
    
    # from github example:
    $curl -L https://api.github.com/repos/octokit/octokit.rb/tarball > octokit.tar.gz

You can then `tar xpvf master`, getting the full archive.  
It will create a directory following the naming convention described in the [question you mentioned][3].

No git binary is needed to get an archive from GitHub, thanks to their [download service "Nodeload"][4].

-----

[`ligemer`][5] proposed [in an edit][6] the following example:

**Edit 2016-08-25 - Shell Example With Wget, Variables, and Untar:**

    #!/bin/bash -ex
    
    # arguments:
    # token = $1
    # organization = $2
    # repo name = $3
    # branch = $4

    wget --header="Authorization: token ${1}" --header="Accept:application/vnd.github.v3.raw" -O - https://api.github.com/repos/${2}/${3}/tarball/${4} | tar xz

Call via:

`$ scriptName.sh token my-organization site.com master`

Above command will download and extract the Github folder to the same directory as the script.

---

[Diogo Quintela][7] suggests [in the comments][8]:

> The following example allow the download, extract and cut the top level directory 

    curl -L https://api.github.com/repos/octokit/octokit.rb/tarball | tar xz --strip=1 


  [1]: https://stackoverflow.com/questions/4051803/git-archive-vs-cp
  [2]: https://developer.github.com/v3/repos/contents/#get-archive-link
  [3]: https://stackoverflow.com/questions/6334040/when-i-download-a-zip-from-github-what-is-the-hex-string-at-the-end-of-the-file
  [4]: https://github.com/blog/678-meet-nodeload-the-new-download-server
  [5]: https://stackoverflow.com/users/2085469/ligemer
  [6]: https://stackoverflow.com/review/suggested-edits/13466031
  [7]: https://stackoverflow.com/users/4248833/diogo-quintela
  [8]: https://stackoverflow.com/questions/8377081/github-api-download-zip-or-tarball-link/8378458#comment67958619_8378458
