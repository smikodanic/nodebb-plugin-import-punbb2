nodebb-plugin-import-punbb2
==========================

inpired from http://www.workinprogress.ca/punbb-to-nodebb and forked from https://github.com/patricksebastien/nodebb-plugin-import-punbb to be compatible with [nodebb-plugin-import](https://github.com/akhoury/nodebb-plugin-import)

--------------------------
a PUNBB forum exporter

based on [nodebb-plugin-import-ubb](https://github.com/akhoury/nodebb-plugin-import-ubb)

### What is this?

It's __just__ an exporter of [PUNBB](http://punbb.informer.com/), which provides an API [nodebb-plugin-import](https://github.com/akhoury/nodebb-plugin-import) can understand


### Why is it even a NodeBB plugin?

it doesn't really need to be, nor that you can use it within NodeBB it self, but, having this as a plugin have few benefits:
* a nodebb- namespace, since you can't really use it for anything else
* it can easily `require` NodeBB useful tools
* 

### What does it export?
read carefully:

- ####Users:
    * `_username` YES.
    * `_alternativeUsername` YES. using 'realname', which [nodebb-plugin-import](https://github.com/akhoury/nodebb-plugin-import) will try to use if the username validation fails
    * `_password` NO. PUNBB uses MD5, NodeBB uses base64 I think, so can't do, but if you use [nodebb-plugin-import](https://github.com/akhoury/nodebb-plugin-import) it will generate random passwords and hand them to you so can email them.
    * `_level` (administrator and moderator) NO, but doable check the structure of PUNBB
    * `_joindate` YES, using 'registered'
    * `_website` YES. using 'url' if URL looks valid, it is exported, but it's not checked if 404s
    * `_picture` NO.
    * `_reputation` NO.
    * `_profileviews` NO.
    * `_location` YES. migrated as is, clear text
    * `_signature` YES. migrated as is (HTML -- read the [Markdown note](#markdown-note) below)
    * `_banned` NO.

- ####Categories:
    * `_name` YES
    * `_description` YES

- ####Topics:
    * `_cid` __(or its PUNBB category aka Forum id)__ YES (but if its parent Category is skipped, this topic gets skipped)
    * `_uid` __(or its PUNBB user id)__ YES (but if its user is skipped, this topic gets skipped)
    * `_title` YES
    * `_content` __(or the 'parent-post` content of this topic)__ YES (HTML - read the [Markdown Note](#markdown-note) below)
    * `_timestamp` YES
    * `_pinned` NO
    * `_viewcount` YES

- ####Posts:
    * `_pid` __(or its PUNBB post id)__
    * `_tid` __(or its PUNBB parent topic id)__ YES (but if its parent topic is skipped, this post gets skipped)
    * `_uid` __(or its PUNBB user id)__ YES (but if its user is skipped, this post is skipped)
    * `_content` YES (HTML - read the [Markdown Note](#markdown-note) below)
    * `_timestamp` YES

### Versions tested on:
  - PUNBB > 1.3.2

### Markdown note

read [nodebb-plugin-import#markdown-note](https://github.com/akhoury/nodebb-plugin-import#markdown-note)

### It's an exporter, why does it have 'import' in its title

To keep the namespacing accurate, this __exporter__ is designed to export data for [nodebb-plugin-import](https://github.com/akhoury/nodebb-plugin-import) only, also for a 1 time use, so why do you care.

