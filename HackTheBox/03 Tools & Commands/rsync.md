The changes that need to get transfered are called deltas. Using deltas to update files is an extremely efficient way to reduce the required bandwidth for the transfer as well as the required time for the transfer to complete.

The official definition of rsync according to the Linux manual page is:

Rsync is a fast and extraordinarily versatile file copying tool. It can copy locally, to/from another host over any remote shell, or to/from a remote rsync daemon. It offers a large number of options that control every aspect of its behavior and permit very flexible specification of the set of files to be copied. It is famous for its deltatransfer algorithm, which reduces the amount of data sent over the network by sending only the differences between the source files and the existing files in the 
destination. Rsync is widely used for backups and mirroring and as an improved copy command for everyday use.

It follows directly from the definition of rsync that it's a great tool for creating/maintaining backups and keeping remote machines in sync with each other. Both of these functionalities are commonly implemented in corporate environment. In these environments time is of of the most importance, so rsync is preferred due to the speedup it offers for these tasks.

The main stages of an rsync transfer are the following:

1. rsync establishes a connection to the remote host and spawns another rsync receiver process.

2. The sender and receiver processes compare what files have changed.

3. What has changed gets updated on the remote host.

It often happens that rsync is misconfigured to permit anonymous login, which can be exploited by an attacker to get access to sensitive information stored on the remote machine. Synced is a Linux box that exposes a directory over rsync with anonymous login. We are able to remotely access this directory using the command line tool rsync and retrieve the flag.

What is rsync? As we have already mentioned rsync is an open source utility that provides fast incremental file transfer. The way rsync works makes it an excellent choice when there is a need to synchronize files between a computer and a storage drive and across networked computers. Because of the flexibility and speed it offers, it has become a standard Linux utility, included in all popular Linux distribution by default. More information about rsync can be found at the Wikipedia page.

Connecting to rsync 
Since rsync is already pre-installed on almost all Linux distributions, we can start interacting with the remote machine.

The generic syntax used by rsync is the following:

rsync [OPTION] … [USER@]HOST::SRC [DEST]

where SRC is the file or directory (or a list of multiple files and directories) to copy from, DEST is the file or directory to copy to, and square brackets indicate optional parameters.

The [OPTION] portion of the syntax, refers to the available options in rsync . The list with all valid options is available over at the official manual page of rsync under the section Options Summary .

The [USER@] optional parameter is used when we want to access the the remote machine in an authenticated way. In this case, we don't have any valid credentials at our disposal so we will omit this portion and try an anonymous authentication.

