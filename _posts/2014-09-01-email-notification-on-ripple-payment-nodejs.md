---
layout: post
title: "Email Notification on Ripple payment Node.js"
description: "Email Notification on Ripple payment Node.js"
category: blogging
tags: (Ripples. XRP, Node.js, ripple-lib)
---
{% include JB/setup %}

# Using Node.js, [Mailgun](http://www.mailgun.com) and [Ripple](http://www.ripple.com) for Ripple Payment Mail Notification

We will write two [Node.js](http://nodejs.org) modules which scan the Ripple Ledger for all transactions, look for a specified account 
and write a mail with the 2nd (mail) modules, which utilizes [Mailgun(-js)](http://www.mailgun.com)

# Prerequisites

* One or better two Ripple accounts (using [Bitstamp](http://www.bitstamp.com) for XRP acquisition makes Ripples account creation quite easy)
* Installed Node.js

# Setup

* Install [mailgun-js](https://www.npmjs.org/package/mailgun-js)
* Install [ripple-lib](https://github.com/ripple/ripple-lib/blob/develop/README.md#installation)

# Configuration

* Note the API key and subdomain of your mailgun account
<img src="/assets/2014-09-01-email-notification-on-ripple-payment-nodejs/img/mailgun.png" width="60%" height="60%"/>
* Note the Ripple accounts public addresses
<img src="/assets/2014-09-01-email-notification-on-ripple-payment-nodejs/img/ripple_recv.png" width="50%" height="50%"/>

# Code

* Start the following Javascript file with node  ``node file.js``

<pre>
// Ripple account to watch for payment
var acct = 'rDPe2PCQtxngg5euANuGiL9o9XEsNfim2g'

// API key for Mailgun
var api_key = 'key-';

// Subdomain for Mailgun
var domain = 'domain.mailgun.org';

// create Mailgun
var Mailgun = require('mailgun-js');
var mailgun = new Mailgun({apiKey: api_key, domain: domain});

// create Ribble lib
var Remote = require('ripple-lib').Remote;
var remote = new Remote({
  servers: [ 'wss://s1.ripple.com:443' ]
});
remote.connect(function() {

	// listen for transaction_all events
	remote.on('transaction_all', function on(transaction) {
		var trx = transaction.transaction

		// is Destination set?
		if (trx.Destination) {
			console.log(trx.Account + " ==> " + trx.Destination + " [" + trx.Amount + "]")
			
			// send mail if account address matches
			if (trx.Destination == acct) {
				var data = {
				  from: 'XRP Watcher', // add valid email address here
				  to: 'me@web.de',
				  subject: 'XRP received',
				  text: 'You received ' + trx.Amount + ' from Account ' + trx.Account
				}
				mailgun.messages().send(data, function (error, body) {
				  console.log(body);
				});
			}
		}
    });
});
</pre>