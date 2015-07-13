..
.. NB:  This file is machine generated, DO NOT EDIT!
..
.. Edit vmod.vcc and run make instead
..

==============
vmod_directors
==============

-------------------------
Backend traffic directors
-------------------------

:Manual section: 3

SYNOPSIS
========

import directors [from "path"] ;


DESCRIPTION
===========

`vmod_directors` enables backend load balancing in Varnish.

The module implements a set of basic load balancing techniques, and
also serves as an example on how one could extend the load balancing
capabilities of Varnish.

To enable load balancing you must import this vmod (directors) in your
VCL:::

  import directors;

Then you define your backends. Once you have the backends declared you
can add them to a director. This happens in executed VCL code. If you
want to emulate the previous behavior of Varnish 3.0 you can just
initialize the directors in vcl_init, like this:::

    sub vcl_init {
        new vdir = directors.round_robin();
        vdir.add_backend(backend1);
        vdir.add_backend(backend2);
    }

As you can see there is nothing keeping you from manipulating the
directors elsewhere in VCL. So, you could have VCL code that would
add more backends to a director when a certain URL is called.

Note that directors can use other directors as backends.

CONTENTS
========

* :ref:`obj_fallback`
* :ref:`func_fallback.add_backend`
* :ref:`func_fallback.backend`
* :ref:`obj_hash`
* :ref:`func_hash.add_backend`
* :ref:`func_hash.backend`
* :ref:`obj_random`
* :ref:`func_random.add_backend`
* :ref:`func_random.backend`
* :ref:`obj_round_robin`
* :ref:`func_round_robin.add_backend`
* :ref:`func_round_robin.backend`

.. _obj_round_robin:

Object round_robin
==================


Description
        Create a round robin director.

	This director will pick backends in a round robin fashion.
Example
	new vdir = directors.round_robin();

.. _func_round_robin.add_backend:

VOID round_robin.add_backend(BACKEND)
-------------------------------------

Prototype
	VOID round_robin.add_backend(BACKEND)

Description
       Add a backend to the round-robin director.
Example
       vdir.add_backend(backend1);
       vdir.add_backend(backend2);

.. _func_round_robin.backend:

BACKEND round_robin.backend()
-----------------------------

Prototype
	BACKEND round_robin.backend()

Description
       Pick a backend from the director.
Example
       set req.backend_hint = vdir.backend();


.. _obj_fallback:

Object fallback
===============


Description
        Create a fallback director.

        A fallback director will try each of the added backends in turn,
        and return the first one that is healthy.

Example
        new vdir = directors.fallback();

.. _func_fallback.add_backend:

VOID fallback.add_backend(BACKEND)
----------------------------------

Prototype
	VOID fallback.add_backend(BACKEND)

Description
        Add a backend to the director.

	Note that the order in which this is done matters for the fallback
	director.

Example
	vdir.add_backend(backend1);
	vdir.add_backend(backend2);

.. _func_fallback.backend:

BACKEND fallback.backend()
--------------------------

Prototype
	BACKEND fallback.backend()

Description
       Pick a backend from the director.
Example
       set req.backend_hint = vdir.backend();


.. _obj_random:

Object random
=============


Description
	Create a random backend director.

	The random director distributes load over the backends using
	a weighted random probability distribution.

Example
	new vdir = directors.random();

.. _func_random.add_backend:

VOID random.add_backend(BACKEND, REAL)
--------------------------------------

Prototype
	VOID random.add_backend(BACKEND, REAL)

Description
	Add a backend to the director with a given weight.

	Each backend backend will receive approximately
	100 * (weight / (sum(all_added_weights))) per cent of the traffic sent
	to this director.

Example
	vdir.add_backend(backend1, 10);
	vdir.add_backend(backend2, 5);
	# 2/3 to backend1, 1/3 to backend2.


.. _func_random.backend:

BACKEND random.backend()
------------------------

Prototype
	BACKEND random.backend()

Description
	Pick a backend from the director.
Example
	set req.backend_hint = vdir.backend();

.. _obj_hash:

Object hash
===========


Description
	Create a hashing backend director.

	The director chooses the backend server by computing a hash/digest of
	the string given to .backend().

	Commonly used with ``client.identity`` or a session cookie to get
	sticky sessions.

Example
	new vdir = directors.hash();

.. _func_hash.add_backend:

VOID hash.add_backend(BACKEND, REAL)
------------------------------------

Prototype
	VOID hash.add_backend(BACKEND, REAL)

Description
	Add a backend to the director with a certain weight.

	Weight is used as in the random director. Recommended value is
	1.0 unless you have special needs.

Example
	vdir.add_backend(backend1, 1.0);
	vdir.add_backend(backend2, 1.0);


.. _func_hash.backend:

BACKEND hash.backend(STRING_LIST)
---------------------------------

Prototype
	BACKEND hash.backend(STRING_LIST)

Description
	Pick a backend from the backend director.

	Use the string or list of strings provided to pick the backend.
Example
	set req.backend_hint = vdir.backend(req.http.cookie);  # pick a backend based on the cookie header from the client

