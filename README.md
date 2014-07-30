nodebb-plugin-import-punbb
==========================

*SEE this link for a step-by-step export:*
http://www.workinprogress.ca/punbb-to-nodebb/


--------------------------
a PUNBB forum exporter to import-ready files.

based on [nodebb-plugin-import-ubb](https://github.com/akhoury/nodebb-plugin-import-ubb)
into this plugin to work along with [nodebb-plugin-import](https://github.com/akhoury/nodebb-plugin-import).

### What is this?

It's __just__ an exporter of [PUNBB](http://punbb.informer.com/), into files that [nodebb-plugin-import](https://github.com/akhoury/nodebb-plugin-import) can understand
and import to NodeBB's database. So, it's not really a conventional nodebb-plugin, and you have to run it from the command line.

### Why is it even a NodeBB plugin?

it doesn't really need to be, nor that you can use it within NodeBB it self, but, having this as a plugin have few benefits:
* a nodebb- namespace, since you can't really use it for anything else
* it can easily `require` NodeBB useful tools, currently, it uses its [util.js](https://github.com/designcreateplay/NodeBB/blob/master/public/src/utils.js) for example.
* potentially, in the future, this plugin, __nodebb-plugin-import-punbb__ can interact with [nodebb-plugin-import](https://github.com/akhoury/nodebb-plugin-import) for a better UX

### Usage

```
cd NodeBB
npm install nodebb-plugin-import-punbb
cd node_modules/nodebb-plugin-import-punbb/bin
node export.js --storage="$HOME/Desktop/storage" --config="../export.config.json" --log="debug,info,warn" --flush
```

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

### I exported, now what?

now use this [nodebb-plugin-import](https://github.com/akhoury/nodebb-plugin-import) to import your files into NodeBB's database

### Versions tested on:
  - PUNBB > 1.3.2

### You configs are required

But you can override the log, storageDir and clearStorage ones with flags when using [bin/export.js](bin/export.js)
```
{
	"log": "debug",
	"storageDir": "../storage",

	"clearStorage": false,

	"db": {
		"host": "localhost",
		"user": "punbb_user",
		"password": "password",
		"database": "punbb_test"
	},
	"tablePrefix": "punbb_"

}
```

### Markdown note

read [nodebb-plugin-import#markdown-note](https://github.com/akhoury/nodebb-plugin-import#markdown-note)

### It's an exporter, why does it have 'import' in its title

To keep the namespacing accurate, this __exporter__ is designed to export data for [nodebb-plugin-import](https://github.com/akhoury/nodebb-plugin-import) only, also for a 1 time use, so why do you care.

