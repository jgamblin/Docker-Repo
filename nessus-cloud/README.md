##  Nessus scanner for cloud.tenable.com
I built a quick dockerfile to create a nessus scanner and connect it to a cloud.tenable.com account since I couldn't find one that worked the way I wanted it to.

### Building

 - Copy your linking key into the  `NESSUS_KEY` from [here](https://cloud.tenable.com/app.html#/scans/scanners).
 - By default adds a a user called `admin` with the password of `admin`.  
  *(You should change this in add_user.exp before you build the container)*

To build you only need to run:
`docker build -t nessus-cloud`

### Running
Running is as simple as:  `docker run -t --name nessus-cloud -p 8834:8834 nessus-cloud`

You should then see a linked scanner in your [cloud.tenable.com](https://cloud.tenable.com) account.

You can also access the local interface at https://127.0.0.1:8834

### Important Notice
I likely don't know what I am doing and this could be done faster, better and simpler some other way. These scripts could also break your network and make you cry.
