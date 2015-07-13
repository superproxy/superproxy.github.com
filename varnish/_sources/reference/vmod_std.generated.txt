..
.. NB:  This file is machine generated, DO NOT EDIT!
..
.. Edit vmod.vcc and run make instead
..

========
vmod_std
========

-----------------------
Varnish Standard Module
-----------------------

:Manual section: 3

SYNOPSIS
========

import std [from "path"] ;


DESCRIPTION
===========

Vmod_std contains basic functions which are part and parcel of Varnish,
but which for reasons of architecture fit better in a VMOD.

One particular class of functions in vmod_std is the conversions functions
which all have the form::

	TYPE type(STRING, TYPE)

These functions attempt to convert STRING to the TYPE, and if that fails,
they return the second argument, which must have the given TYPE.

CONTENTS
========

* :ref:`func_collect`
* :ref:`func_duration`
* :ref:`func_fileread`
* :ref:`func_healthy`
* :ref:`func_integer`
* :ref:`func_ip`
* :ref:`func_log`
* :ref:`func_port`
* :ref:`func_querysort`
* :ref:`func_random`
* :ref:`func_real`
* :ref:`func_real2time`
* :ref:`func_set_ip_tos`
* :ref:`func_syslog`
* :ref:`func_time2integer`
* :ref:`func_time2real`
* :ref:`func_timestamp`
* :ref:`func_tolower`
* :ref:`func_toupper`

.. _func_toupper:

STRING toupper(STRING_LIST)
---------------------------

Prototype
	STRING toupper(STRING_LIST)

Description
	Converts the string *s* to upper case.
Example
	set beresp.http.x-scream = std.toupper("yes!");

.. _func_tolower:

STRING tolower(STRING_LIST)
---------------------------

Prototype
	STRING tolower(STRING_LIST)

Description
	Converts the string *s* to lower case.
Example
	set beresp.http.x-nice = std.tolower("VerY");

.. _func_set_ip_tos:

VOID set_ip_tos(INT)
--------------------

Prototype
	VOID set_ip_tos(INT)

Description
	Sets the Type-of-Service flag for the current session. Please
	note that the TOS flag is not removed by the end of the
	request so probably want to set it on every request should you
	utilize it.
Example
	| if (req.url ~ ^/slow/) {
	|    std.set_ip_tos(0x0);
	| }

.. _func_random:

REAL random(REAL, REAL)
-----------------------

Prototype
	REAL random(REAL, REAL)

Description
	Returns a random REAL number between *a* and *b*.
Example
	set beresp.http.x-random-number = std.random(1, 100);

.. _func_log:

VOID log(STRING_LIST)
---------------------

Prototype
	VOID log(STRING_LIST)

Description
	Logs *string* to the shared memory log, using VSL tag *SLT_VCL_Log*.
Example
	std.log("Something fishy is going on with the vhost " + req.host);

.. _func_syslog:

VOID syslog(INT, STRING_LIST)
-----------------------------

Prototype
	VOID syslog(INT, STRING_LIST)

Description
	Logs *string* to syslog marked with *priority*.  See your
	system's syslog.h file for the legal values of *priority*.
Example
	std.syslog(8 + 1, "Something is wrong");

.. _func_fileread:

STRING fileread(PRIV_CALL, STRING)
----------------------------------

Prototype
	STRING fileread(PRIV_CALL, STRING)

Description
	Reads a file and returns a string with the content. Please
	note that it is not recommended to send variables to this
	function the caching in the function doesn't take this into
	account. Also, files are not re-read.
Example
	set beresp.http.x-served-by = std.fileread("/etc/hostname");

.. _func_collect:

VOID collect(HEADER)
--------------------

Prototype
	VOID collect(HEADER)

Description
	Collapses the header, joining the headers into one.
Example
	std.collect(req.http.cookie);
	This will collapse several Cookie: headers into one, long
	cookie header.

.. _func_duration:

DURATION duration(STRING, DURATION)
-----------------------------------

Prototype
	DURATION duration(STRING, DURATION)

Description
	Converts the string *s* to seconds. *s* must be quantified
	with ms (milliseconds), s (seconds), m (minutes), h (hours),
	d (days), w (weeks) or y (years) units. If *s* fails to parse,
	*fallback* will be returned.
Example
	set beresp.ttl = std.duration("1w", 3600s);

.. _func_integer:

INT integer(STRING, INT)
------------------------

Prototype
	INT integer(STRING, INT)

Description
	Converts the string *s* to an integer.  If *s* fails to parse,
	*fallback* will be returned.
Example
	if (std.integer(beresp.http.x-foo, 0) > 5) { ... }

.. _func_ip:

IP ip(STRING, IP)
-----------------

Prototype
	IP ip(STRING, IP)

Description
	Converts string *s* to the first IP number returned by
	the system library function getaddrinfo(3).  If conversion
	fails, *fallback* will be returned.
Example
	if (std.ip(req.http.X-forwarded-for, "0.0.0.0") ~ my_acl) { ... }

.. _func_real:

REAL real(STRING, REAL)
-----------------------

Prototype
	REAL real(STRING, REAL)

Description
	Converts the string *s* to a real.  If *s* fails to parse,
	*fallback* will be returned.
Example
	set req.http.x-real = std.real(req.http.x-foo, 0.0);

.. _func_real2time:

TIME real2time(REAL)
--------------------

Prototype
	TIME real2time(REAL)

Description
	Converts the real *r* to a time.
Example
	set req.http.x-time = std.real2time(1140618699.00);

.. _func_time2integer:

INT time2integer(TIME)
----------------------

Prototype
	INT time2integer(TIME)

Description
	Converts the time *t* to a integer.
Example
	set req.http.x-int = std.time2integer(now);

.. _func_time2real:

REAL time2real(TIME)
--------------------

Prototype
	REAL time2real(TIME)

Description
	Converts the time *t* to a real.
Example
	set req.http.x-real = std.time2real(now);

.. _func_healthy:

BOOL healthy(BACKEND)
---------------------

Prototype
	BOOL healthy(BACKEND)

Description
	Returns true if the backend is healthy.

.. _func_port:

INT port(IP)
------------

Prototype
	INT port(IP)

Description
	Returns the port number of an IP address.

.. _func_timestamp:

VOID timestamp(STRING)
----------------------

Prototype
	VOID timestamp(STRING)

Description
	Introduces a timestamp in the log with the current time. Uses
	the argument as the timespamp label. This is useful to time
	the execution of lengthy VCL procedures, and makes the
	timestamps inserted automatically by Varnish more accurate.
Example
	std.timestamp("curl-request");

.. _func_querysort:

STRING querysort(STRING)
------------------------

Prototype
	STRING querysort(STRING)

Description
        Sorts the querystring for cache normalization purposes.

Example
        set req.url = std.querysort(req.url);



SEE ALSO
========

* vcl(7)
* varnishd(1)

HISTORY
=======

The Varnish standard module was released along with Varnish Cache 3.0.
This manual page was written by Per Buer with help from Martin Blix
Grydeland.

COPYRIGHT
=========

This document is licensed under the same licence as Varnish
itself. See LICENCE for details.

