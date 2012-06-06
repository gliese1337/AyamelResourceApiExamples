# Resource Examples README #

The documents contained in this directory are example JSON/YAML files represent example resoponses from queries to the Ayamel Resource Library API.  Specific examples are given for common use-cases, such as uploaded files and Youtube videos.  For documentation on the API for consuming and manipulating resource objects, refer to the online documentation at [http://ayamel.americancouncils.org/index.php](http://ayamel.americancouncils.org/index.php);

## Usage ##

You can refer to the documents contained in this directory for documentation on current structures.  The example files should be usable by programs to simulate example API responses for testing purposes.

## Contributing ##

To modify the documents contained in this directory, fork the repository on `github`, make your changes, and submit pull requests.  This ensures that the most recent documentation is available for testing purposes.

## Todo ##

The data model is not yet completely finalized.  There are several issues left to be addressed:

* l2Data field, a sub object with several fields... what are they?
* Age-appropriate rating/restriction field of some sort

# General Resource Object Structure #

The main structure of objects follows a consisten format as defined below.  Sub objects are described in sub sections. Keys in bold are allowed to be set by a contributing client application, all others are determined by the server and read-only.

## Top level ##

These are the base fields associated with all resources at the top level.  Some are determined by the server, and are thus read-only.

* `id`: a unique identifier for the resource
* **title**: the human-readable name of the resource
* **description**: a short human-readable description of the resource
* **keywords**: a string of keyword entries as entered by a user or another application
* **categories**: an array of category labels
* **type**: a generic label for the kind of resource that was uploaded, corresponding with the primary representation type (video, audio, image, document, timed text, playlist, etc); needed for transcoding and search purposes
* **public**: boolean whether or not the resource is visible to clients other than the one which originally contributed the resource
* `dateAdded`: Date/Time string: when an object was added into the library
* `dateModified`: Date/Time string: last time an object was modified
* `dateDeleted`: Date/Time string IF an object has been deleted
* `status`: one of an enumerated set of status strings, potential values include
	 * `ok`: used when status is normal
	 * `deleted`: used when an object has been deleted
	 * `processing`: used if the content for an object is currently being processed asynchronously (this locks the object from being modified)
	 * `awaiting_content`: used when an object has no content entries
	 * `awaiting_processing`: used when the content of an object is in a queue to be processed, for example if an uploaded file is waiting to be transcoded into multiple formats
* **copyright**: string with copyright information
* **license**: string with license information
* **origin**: See [origin](#origin) subheading; an object describing information about the origin of the resource
* **client**: See [client](#client) subheading; an object describing information about the contributer of the object to the library
* **l2Data**: See [l2Data](#l2Data) subheading; an object describing linguistically relevant metadata
* `content`: See [content](#content) subheading; an object describing how to access the actual resource data
* **relations**: See [relations](#relations) subheading; an array of relation objects

### [origin] ###

This object is optional, but can be used by a client to describe information about where a resource originated.  Depending on the type of content specified by a resources, this field could automatically be populated by the server.  All values are optional strings, not necessarily meant for program consumption, as an original resource may or may not be in a digital format.

* **creator**: a string person, or reference to a person
* **location**: a string reference to a location
* **date**: a string description of a date
* **format**: a string description of the original format of the resource, which may or may not be digital
* **note**: a string text note
* **uri**: a properly formatted string URI, consumable by a program

### [client] ###

This object describes a client contributer of the Resource Library.  Not all values will be present for all contributing systems.

* `id`: The string id of a client system as it is known to the Resource Library
* `name`: A string human-readable name of a client system, if applicable
* `uri`: A string URI to a client system, if applicable
* **user**: A string reference to a specific user of a client system, if applicable
* **userUri**: A string URI that references a specific user of a client system, if applicable

### [l2Data] ###

	throw new RuntimeException("THIS NEEDS WORK, WE DON'T KNOW WHAT WE'RE DOING WITH THIS YET, AND IT'S CRITICAL.");
	
### [content] ###

This object describes the actual representations of the content, as it can be consumed by a program.  This includes references to files, possibly multiple files of varying type and quality, as well as potentially information about how to embed a resource directly into another document.

* **files**: See [file](#file) subheading; an array of file reference objects
* **oembed**: A JSON object describing embed information, as defined by `http://oembed.com`
* **canonicalUri**: A string URI reference to a sever with advanced content negotiation capabilities

#### [file] ####

* **downloadUri**: A string uri for a program to consume in order to download a file
* **streamUri**: A string reference to a streaming server which will stream the file
* **mime**: A string mime, as precise as can be determined
* **representation**: a string describing the relation of this representation to the canonical resource; format "type;quality", where type is one of "transcoded", "summary", or "original", and quality is a floating-point number of up to 4 digits.
* **attributes**: a key/value hash of attributes about a file, the contents of which depend upon the mime

		This needs work, specific examples of files and attributes are needed.

### relations ###

This field is an array of `relation` objects, which describe ways this particular resource relates to other resources.

* **subjectId**: string id of subject resource object
* **objectId**: string id of object resource
* **type**: A string key enumerating the type of relation, as the `subjectId` relates the `objectId`. Can be one of a list of possible valid values
	
		NEEDS WORK, WHAT ARE THE POSSIBLE VALUES?
	
* **attributes**: a key/val hash of relevant values, which depend on the type of relation specified in the field above

# Examples Resources #

This directory contains example json files that can serve as example documentation, and testing stubs for API responses.

* Video upload ([json](example.video.json)),  - An example resource uploaded by a client application
* Youtube video ([json](example.youtube.json)) - An example Youtube video contributed by a client application
* TODO: Video transcript ([json](example.transcript.json)) - An example transcript of the above video
* TODO: A Flickr image ([json](example.flickr.json)) - An example Flickr image contributed by a client application
* TODO: A podcast ([json](example.podcast.json)) - An example podcast available somewhere online
* TODO: A podcast entry ([json](example.podcast_entry.json)) - An example podcast entry available somewhere online