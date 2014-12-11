# Shelly.js

## What is it? 

Build queries and update MongoDB documents quickly. Written in JS for MongoDB's JS Shell.

## Why did you built it? 

I just got tired of writing really difficult queries for updating a few records. I got tired of hitting the shell window width and flubbing bracket placement. 

## Example use.. 

### Find a single record by id

Mongo
	
	db.contacts.find({ _id : ObjectId('5489ea63c96a880167462784') })`

Shelly
	
	var c = Shelly('contacts')
	c.find('5489ea63c96a880167462784')

	//or
	c.first('5489ea63c96a880167462784')

### Find records that have 'triggers' key

Mongo
	
	db.contacts.find({ 'triggers' : { $exists : true } })

Shelly
	
	var c = Shelly('contacts')
	c.has('triggers')
	c.each...

### Updating a records

Mongo
	
	db.contacts.update({ 'triggers.email' : { $exists : false } }, { 
		$set : { 'triggers.email' : true }
	}, { $multi : true });

Shelly
	
	var c = Shelly('contacts');
	c.has('triggers.email', false);
	c.set('triggers.email', true);
	c.update({ multi : true });

## Other Nice Stuff

Inspect your current state

	var c = Shelly('contacts');
	c.where(query)
	c.and_where(query)
	
	//what does that look like?
	c.inspect()
	//oh yeah!

get/set values out of objects

	var c = Shelly('contacts')
	c.where(query)
	c.get()
	c.forEach(function(contact){
		var path = 'meta.phones.mobile.0';
		var mobile = contact.get('meta.emails.mobile.0');
		mobile =  mobile ? myFormatter(mobile) : 'some other value';
		contact.set(path, mobile);
		contact.save();
	});

## Why no chaining? 

Because this will be printed to the shell. If you know a way to mute this
let me know!! 

## TODO

- Support for and/or conjections with and_where or_where
- Support building queries with other Shelly queries
- map reduce, stuff
- Easy join builder