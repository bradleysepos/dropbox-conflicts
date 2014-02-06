dropbox-conflicts
=================

Find Dropbox conflicts. Requires Mac OS X 10.8 or greater.

## Introduction

When a file in Dropbox is changed on two separate machines (e.g. two people simultaneously edit and save a shared document), Dropbox may not be able to sort out the differences. Rather than let one set of changes clobber the other, Dropbox preserves each machine's data by placing additional "conflicted copies" of the file in the same directory. A human must then inspect these conflicts and decide whether to keep, delete, or merge changes from the additional files.

In short, `dropbox-conflicts` makes finding Dropbox conflicts easy and with the help of launchd, even automatic.

## Installation

1. Copy `dropbox-conflicts` anywhere on your `PATH`
2. Make it executable: `$ chmod +x /path/to/dropbox-conflicts`
3. Install [terminal-notifier] for notifications: `$ sudo gem install terminal-notifier`

## Configuration

Make sure `PATH` is properly set for both `bash` and launchd. One way of accomplishing this is to customize the following and add to `~/.bash_profile`:

```
export PATH=$PATH:/another/path:/still/yet/another  
launchctl setenv PATH $PATH
```

If your Dropbox is located somewhere other than `~/Dropbox`, you can similarly make use of the `DROPBOX_DIR` environment variable:

```
export DROPBOX_DIR=/path/to/Dropbox  
launchctl setenv DROPBOX_DIR $DROPBOX_DIR
```

Or create a symlink at `~/Dropbox`:

```
$ cd ~ && ln -s /path/to/Dropbox
```

You can also pass the path specifically using the `-d` flag:

```
$ dropbox-conflicts -d /path/to/Dropbox
```

## Usage

```
$ dropbox-conflicts [-hv]  
$ dropbox-conflicts [-fnq] [-d path]
```

`-h`  display help  
`-v`  display version  
`-d`  specify directory  
`-f`  force display notification (implies -n)  
`-n`  enable notification  
`-q`  quiet mode, do not print result

### Examples

Find and print a list of all conflicts, suitable for piping:

```
$ dropbox-conflicts
```

Display a notification if conflicts are found (will not repeat notify):

```
$ dropbox-conflicts -n
```

Same as above but do not print to standard output:

```
$ dropbox-conflicts -nq
```

Force display a notification if conflicts are found:

```
$ dropbox-conflicts -f
```

## Notifications

Enabling notification with `-n` will display a notification in Notification Center using [terminal-notifier], but only when conflicts are found and something has changed since the last notification.

The `-f` flag will force a notification display when conflicts are found regardless of whether something has changed since the last notification.

### Automatic notifications

With a little effort, you can automatically receive notifications when something changes.

To periodically run `dropbox-conflicts` in the background:

1. Copy `com.bradleysepos.dropbox-conflicts.plist` from `Extras` to `~/Library/LaunchAgents`
2. Run `$ launchctl load ~/Library/LaunchAgents/com.bradleysepos.dropbox-conflicts.plist`

The default is to check every 15 minutes (900 seconds) and can be customized by editing the `StartInterval` key.

### Saved search

When notification is enabled, `dropbox-conflicts` creates a temporary Saved Search, also known as a Smart Folder, in order to display results in a Finder window. Look for the template `Dropbox Conflicts.savedSearch` in `Extras`.

## Troubleshooting

Problems with automatic notifications? Check your launchd environment with `$ launchctl getenv PATH`. See Installation for more.

`dropbox-conflicts` returns exit code `0` (true) when no conflicts are found and no errors are encountered.

## License

Copyright 2014 Bradley Sepos

Released under the MIT License. See [LICENSE] for details.

[terminal-notifier]: https://github.com/alloy/terminal-notifier
[LICENSE]: https://github.com/bradleysepos/dropbox-conflicts/blob/master/LICENSE