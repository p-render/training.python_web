Python Web Programming
======================

.. image:: img/python.png
    :align: left
    :width: 33%

Session 1: Networking and Sockets

.. class:: intro-blurb

Wherein we learn about the basic structure of the internet and explore the
building blocks that make it possible.


But First
---------

Class presentations are available online for your use

http://github.com/cewing/training.python_web

Use the ``week-long-format`` branch for this class

Licensed with Creative Commons BY-NC-SA

* You must attribute the work
* You may not use the work for commercial purposes
* You have to share your versions just like this one

Find mistakes? See improvements? Make a pull request.


But First
---------

Daily Schedule

* 9:00 am to 10:30 am - Session 1a

* 15 minute break

* 10:45 am to 12:30 pm - Session 1b

* 1 hour lunch

* 1:30 pm to 3:00 pm - Session 2a

* 15 minute break

* 3:15 pm to 5:00 pm - Session 2b


But First
---------

Classroom Protocol

.. class:: incremental

Question to ask:

.. class:: incremental

* What did you just say?
* Please explain what we just did again?
* Why didn't that work for me?
* Is that a typo?


But First
---------

Classroom Protocol

.. class:: incremental

Question **not** to ask:

.. class:: incremental

* Hypotheticals: What happens if I do X?
* Research: Can Python do Y?
* Syllabus: Are we going to cover Z in class?
* Marketing questions: please just don't.
* Performance questions: Is Python fast enough?
* Unpythonic: Why doesn't Python do it some other way?
* Show off: Look what I just did!


But First
---------

.. class:: big-centered

Introductions


Computer Communications
-----------------------

.. image:: img/network_topology.png
    :align: left
    :width: 40%

.. class:: incremental

* processes can communicate

* inside one machine

* between two machines

* among many machines

.. class:: image-credit

image: http://en.wikipedia.org/wiki/Internet_Protocol_Suite


Computer Communications
-----------------------

.. image:: img/data_in_tcpip_stack.png
    :align: left
    :width: 55%

.. class:: incremental

* Process divided into 'layers'

* 'Layers' are mostly arbitrary

* Different descriptions have different layers

* Most common is the 'TCP/IP Stack'

.. class:: image-credit

image: http://en.wikipedia.org/wiki/Internet_Protocol_Suite


The TCP/IP Stack - Link
-----------------------

The bottom layer is the 'Link Layer'

.. class:: incremental

* Deals with the physical connections between machines, 'the wire'

* Packages data for physical transport

* Executes transmission over a physical medium

  * what that medium is is arbitrary

* Primarily uses the Network Interface Card (NIC) in your computer


The TCP/IP Stack - Internet
---------------------------

Moving up, we have the 'Internet Layer'

.. class:: incremental

* Deals with addressing and routing

  * Where are we going and how do we get there?

* Agnostic as to physical medium (IP over Avian Carrier - IPoAC)

* Makes no promises of reliability

* Two addressing systems

  .. class:: incremental

  * IPv4 (current, limited '192.168.1.100')

  * IPv6 (future, 3.4 x 10^38 addresses, '2001:0db8:85a3:0042:0000:8a2e:0370:7334')


The TCP/IP Stack - Internet
---------------------------

.. class:: big-centered

That's 4.3 x 10^28 addresses *per person alive today*


The TCP/IP Stack - Transport
----------------------------

Next up is the 'Transport Layer'

.. class:: incremental

* Deals with transmission and reception of data

  * error correction, flow control, congestion management

* Common protocols include TCP & UDP

  * TCP: Tranmission Control Protocol

  * UDP: User Datagram Protocol

* Not all Transport Protocols are 'reliable'

  .. class:: incremental

  * TCP ensures that dropped packets are resent

  * UDP makes no such assurance
  
  * Reliability is slow and expensive


The TCP/IP Stack - Transport
----------------------------

The 'Transport Layer' also establishes the concept of a **port**

.. class:: incremental

* IP Addresses designate a specific *machine* on the network

* A **port** provides addressing for individual *applications* in a single host

* 192.168.1.100:80  (the *:80* part is the **port**)

.. class:: incremental

This means that you don't have to worry about information intended for your
web browser being accidentally read by your email client.


The TCP/IP Stack - Transport
----------------------------

There are certain **ports** which are commonly understood to belong to given
applications or protocols:

.. class:: incremental

* 80/443 - HTTP/HTTPS
* 20 - FTP
* 22 - SSH
* 23 - Telnet
* 25 - SMTP
* ...

.. class:: small

(see http://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers)


The TCP/IP Stack - Transport
----------------------------

Ports are grouped into a few different classes

.. class:: incremental

* Ports numbered 0 - 1023 are *reserved* 

* Ports numbered 1024 - 65535 are *open*

* Ports numbered 49152 - 65535 are generally considered *ephemeral*


The TCP/IP Stack - Application
------------------------------

The topmost layer is the 'Application Layer'

.. class:: incremental

* Deals directly with data produced or consumed by an application

* Reads or writes data using a set of understood, well-defined **protocols**

  * HTTP, SMTP, FTP etc.

* Does not know (or need to know) about lower layer functionality

  * The exception to this rule is **endpoint** data (or IP:Port)


The TCP/IP Stack - Application
------------------------------

.. class:: big-centered

this is where we live and work


Sockets
-------

Think back for a second to what we just finished discussing, the TCP/IP stack.

.. class:: incremental

* The *Internet* layer gives us an **IP Address**

* The *Transport* layer establishes the idea of a **port**.

* The *Application* layer doesn't care about what happens below...

* *Except for* **endpoint data** (IP:Port)

.. class:: incremental

A **Socket** is the software representation of that endpoint.

.. class:: incremental

Opening a **socket** creates a kind of transceiver that can send and/or
receive data at a given IP address and Port.


Sockets in Python
-----------------

Python provides a standard library module which provides socket functionality.
It is called **socket**.  Let's spend a few minutes getting to know this
module.

We're going to do this next part together, so open up a terminal and start a
python interpreter


Sockets in Python
-----------------

The sockets library provides tools for finding out information about hosts on
the network. For example, you can find out about the machine you are currently
using::

    >>> import socket
    >>> socket.gethostname()
    'heffalump.local'
    >>> socket.gethostbyname(socket.gethostname())
    '10.211.55.2'


Sockets in Python
-----------------

You can also find out about machines that are located elsewhere, assuming you
know their hostname. For example::

    >>> socket.gethostbyname('google.com')
    '173.194.33.4'
    >>> socket.gethostbyname('unc.edu')
    '152.19.240.120'
    >>> socket.gethostbyname('crisewing.com')
    '108.59.11.99'


Sockets in Python
-----------------

The ``gethostbyname_ex`` method of the ``socket`` library provides more
information about the machines we are exploring::

    >>> socket.gethostbyname_ex('google.com')
    ('google.com', [], ['173.194.33.9', '173.194.33.14',
                        ...
                        '173.194.33.6', '173.194.33.7',
                        '173.194.33.8'])
    >>> socket.gethostbyname_ex('crisewing.com')
    ('crisewing.com', [], ['108.59.11.99'])
    >>> socket.gethostbyname_ex('www.rad.washington.edu')
    ('elladan.rad.washington.edu', # <- canonical hostname
     ['www.rad.washington.edu'], # <- any machine aliases
     ['128.95.247.84']) # <- all active IP addresses


Sockets in Python
-----------------

To create a socket, you use the **socket** method of the ``socket`` library.
It takes up to three optional positional arguments (here we use none to get
the default behavior)::

    >>> foo = socket.socket()
    >>> foo
    <socket._socketobject object at 0x10046cec0>


Sockets in Python
-----------------

A socket has some properties that are immediately important to us. These
include the *family*, *type* and *protocol* of the socket::

    >>> foo.family
    2
    >>> foo.type
    1
    >>> foo.proto
    0

.. class:: incremental

You might notice that the values for these properties are integers.  In fact, 
these integers are **constants** defined in the socket library.


A quick utility method
----------------------

Let's define a method in place to help us see these constants. It will take a
single argument, the shared prefix for a defined set of constants:

.. class:: small

::

    >>> def get_constants(prefix):
    ...     """mapping of socket module constants to their names."""
    ...     return dict( (getattr(socket, n), n)
    ...                  for n in dir(socket)
    ...                  if n.startswith(prefix)
    ...                  )
    ...
    >>>


Socket Families
---------------

Think back a moment to our discussion of the *Internet* layer of the TCP/IP
stack.  There were a couple of different types of IP addresses:

.. class:: incremental

* IPv4 ('192.168.1.100')

* IPv6 ('2001:0db8:85a3:0042:0000:8a2e:0370:7334')

.. class:: incremental

The *family* of a socket corresponds to the addressing system it uses for
connecting.


Socket Families
---------------

Families defined in the ``socket`` library are prefixed by ``AF_``::

    >>> families = get_constants('AF_')
    >>> families
    {0: 'AF_UNSPEC', 1: 'AF_UNIX', 2: 'AF_INET',
     11: 'AF_SNA', 12: 'AF_DECnet', 16: 'AF_APPLETALK',
     17: 'AF_ROUTE', 23: 'AF_IPX', 30: 'AF_INET6'}

.. class:: small incremental

*Your results may vary*

.. class:: incremental

Of all of these, the ones we care most about are ``2`` (IPv4) and ``30`` (IPv6).


Unix Domain Sockets
-------------------

When you are on a machine with an operating system that is Unix-like, you will
find another generally useful socket family: ``AF_UNIX``, or Unix Domain
Sockets. Sockets in this family:

.. class:: incremental

* connect processes **on the same machine**

* are generally a bit slower than IPC connnections

* have the benefit of allowing the same API for programs that might run on one
  machine __or__ across the network

* use an 'address' that looks like a pathname ('/tmp/foo.sock')


Test your skills
----------------

What is the *default* family for the socket we created just a moment ago?

.. class:: incremental

(remember we bound the socket to the symbol ``foo``)


Socket Types
------------

The socket *type* determines the semantics of socket communications.

Look up socket type constants with the ``SOCK_`` prefix::

    >>> types = get_constants('SOCK_')
    >>> types
    {1: 'SOCK_STREAM', 2: 'SOCK_DGRAM',
     ...}

.. class:: incremental

The most common are ``1`` (TCP type communication) and ``2`` (UDP type
communication).


Test your skills
----------------

What is the *default* type for our generic socket, ``foo``?


Socket Protocols
----------------

A socket also has a designated *protocol*. The constants for these are
prefixed by ``IPPROTO_``::

    >>> protocols = get_constants('IPPROTO_')
    >>> protocols
    {0: 'IPPROTO_IP', 1: 'IPPROTO_ICMP',
     ...,
     255: 'IPPROTO_RAW'}

.. class:: incremental

The choice of which protocol to use for a socket is determined by the
communications protocol you intend to use.  ``IP``? ``ICMP``? ``UDP``?


Test your skills
----------------

What is the *default* protocol used by our generic socket, ``foo``?


Address Information
-------------------

These three properties of a socket correspond to the three positional arguments
you may pass to the constructor.  This allows you to create sockets that have
specific communications profiles::

    >>> bar = socket.socket(socket.AF_INET,
    ...                     socket.SOCK_DGRAM, 
    ...                     socket.IPPROTO_UDP)
    ...
    >>> bar
    <socket._socketobject object at 0x1005b8b40>


Address Information
-------------------

But when you are creating a socket to communicate with a remote service, how
can you determine the *correct* values to use?

.. class:: incremental

You ask.


Client Connections
------------------

The information returned by a call to ``socket.getaddrinfo`` is all you need
to make a proper connection to a socket on a remote host.  The value returned
is a tuple of

.. class:: incremental

* socket family
* socket type
* socket protocol
* canonical name (usually empty, unless requested by flag)
* socket address (tuple of IP and Port)


A quick utility method
----------------------

Again, let's create a utility method in-place so we can see this in action::

    >>> def get_address_info(host, port):
    ...     for response in socket.getaddrinfo(host, port):
    ...         fam, typ, pro, nam, add = response
    ...         print 'family: ', families[fam]
    ...         print 'type: ', types[typ]
    ...         print 'protocol: ', protocols[pro]
    ...         print 'canonical name: ', nam
    ...         print 'socket address: ', add
    ...         print
    ...
    >>>


On Your Own Machine
-------------------

Now, ask your own machine what services are available on 'http'::

    >>> get_address_info(socket.gethostname(), 'http')
    family:  AF_INET
    type:  SOCK_DGRAM
    protocol:  IPPROTO_UDP
    canonical name:  
    socket address:  ('10.211.55.2', 80)
    
    family:  AF_INET
    ...
    >>>

.. class:: incremental

What answers do you get?


On the Internet
---------------

::

    >>> get_address_info('crisewing.com', 'http')
    family:  AF_INET
    type:  SOCK_DGRAM
    ...

    family:  AF_INET
    type:  SOCK_STREAM
    ...
    >>>

.. class:: incremental

Try a few other servers you know about.


First Steps
-----------

.. class:: big-centered

Let's put this to use


Construct a Socket
------------------

We've already made a socket ``foo`` using the generic constructor without any
arguments.  We can make a better one now by using real address information from
a real server online:

.. class:: small

::

    >>> streams = [info
    ...     for info in socket.getaddrinfo('crisewing.com', 'http')
    ...     if info[1] == socket.SOCK_STREAM]
    >>> streams
    [(2, 1, 6, '', ('108.59.11.99', 80))]
    >>> info = streams[0]
    >>> cewing_socket = socket.socket(*info[:3])


Connecting a Socket
-------------------

Once the socket is constructed with the appropriate *family*, *type* and
*protocol*, we can connect it to the address of our remote server::

    >>> cewing_socket.connect(info[-1])
    >>> 

.. class:: incremental

* a successful connection returns ``None``

* a failed connection raises an error

* you can use the *type* of error returned to tell why the connection failed.


Sending a Message
-----------------

Send a message to the server on the other end of our connection (we'll
learn is session 2 about the message we are sending)::

    >>> msg = "GET / HTTP/1.1\r\n"
    >>> msg += "Host: crisewing.com\r\n\r\n"
    >>> cewing_socket.sendall(msg)
    >>>

.. class:: incremental small

* the transmission continues until all data is sent or an error occurs

* success returns ``None``

* failure to send raises an error 

* you can use the type of error to figure out why the transmission failed

* you **cannot** know how much, if any, of your data was sent


Receiving a Reply
-----------------

Whatever reply we get is received by the socket we created. We can read it
back out::

    >>> response = cewing_socket.recv(4096)
    >>> response
    'HTTP/1.1 200 OK\r\nDate: Thu, 03 Jan 2013 05:56:53
    ...

.. class:: incremental small

* The sole required argument is ``buffer_size`` (an integer). It should be a
  power of 2 and smallish
* It returns a byte string of ``buffer_size`` (or smaller if less data was
  received)
* If the response is longer than ``buffer size``, you can call the method
  repeatedly. The last bunch will be less than ``buffer size``.


Cleaning Up
-----------

When you are finished with a connection, you should always close it::

  >>> cewing_socket.close()


Putting it all together
-----------------------

First, connect and send a message:

::

    >>> streams = [info
    ...     for info in socket.getaddrinfo('crisewing.com', 'http')
    ...     if info[1] == socket.SOCK_STREAM]
    >>> info = streams[0]
    >>> cewing_socket = socket.socket(*info[:3])
    >>> cewing_socket.connect(info[-1])
    >>> msg = "GET / HTTP/1.1\r\n\r\n"
    >>> msg += "Host: crisewing.com\r\n\r\n"
    >>> cewing_socket.sendall(msg)


Putting it all together
-----------------------

Then, receive a reply, iterating until it is complete:

::

    >>> buffsize = 4096
    >>> response = ''
    >>> done = False
    >>> while not done:
    ...     msg_part = cewing_socket.recv(buffsize)
    ...     if len(msg_part) < buffsize:
    ...         done = True
    ...         cewing_socket.close()
    ...     response += msg_part
    >>> len(response)
    19427


Break Time
----------

So far we have:

.. class:: incremental

* learned about the "layers" of the TCP/IP Stack
* discussed *families*, *types* and *protocols* in sockets
* learned some API for finding out how to connect to a remote server
* made our first connection to a server
* and sent and received our first messages through a socket.

.. class:: incremental

Not bad for a Monday morning.

.. class:: incremental

Let's take 10 minutes and return to learn about the other end of this wire.


Server Side
-----------

.. class:: big-centered

What about the other half of the equation?

Construct a Socket
------------------

**For the moment, stop typing this into your interpreter.**

.. container:: incremental

    Again, we begin by constructing a socket. Since we are actually the server
    this time, we get to choose family, type and protocol::

        >>> server_socket = socket.socket(
        ...     socket.AF_INET,
        ...     socket.SOCK_STREAM,
        ...     socket.IPPROTO_IP)
        ... 
        >>> server_socket
        <socket._socketobject object at 0x100563c90>


Bind the Socket
---------------

Our server socket needs to be bound to an address. This is the IP Address and
Port to which clients must connect::

    >>> address = ('127.0.0.1', 50000)
    >>> server_socket.bind(address)

.. class:: incremental

**Terminology Note**: In a server/client relationship, the server *binds* to
an address and port. The client *connects*


Listen for Connections
----------------------

Once our socket is bound to an address, we can listen for attempted
connections::

    >>> server_socket.listen(1)

.. class:: incremental

* The argument to ``listen`` is the *backlog*

* The *backlog* is the **maximum** number of connections that the socket will
  queue

* Once the limit is reached, the socket refuses new connections


Accept Incoming Messages
------------------------

When a socket is listening, it can receive incoming messages::

    >>> connection, client_address = server_socket.accept()
    ... # this blocks until a client connects
    >>> connection.recv(16)

.. class:: incremental

* The ``connection`` returned by a call to ``accept`` is a **new socket**

* It is this *new* socket that is used for communications with the client
  socket

* the ``client_address`` is a two-tuple of IP Address and Port for the client
  socket

* The number of *new* sockets that can be spun off by a listening socket is 
  equal to ``backlog``


Send a Reply
------------

The same socket that received a message from the client may be used to return
a reply::

    >>> connection.sendall("message received")


Clean Up
--------

Once a transaction between the client and server is complete, the
``connection`` socket should be closed::

    >>> connection.close()

.. class:: incremental

* Closing the connection socket will decrement the number of active sockets in
  the queue

* If the maximum specified by ``backlog`` had been reached, this will allow a
  new connection to be made.


Getting the Flow
----------------

The flow of this interaction can be a bit confusing.  Let's see it in action
step-by-step.

.. class:: incremental

Open a second python interpreter and place it next to your first so you can
see both of them at the same time.


Create a Server
---------------

In your first python interpreter, create a server socket and prepare it for
connections::

    >>> server_socket = socket.socket(
    ...     socket.AF_INET,
    ...     socket.SOCK_STREAM,
    ...     socket.IPPROTO_IP)
    >>> server_socket.bind(('127.0.0.1', 50000))
    >>> server_socket.listen(1)
    >>> conn, addr = server_socket.accept()
    
.. class:: incremental

At this point, you should **not** get back a prompt. The server socket is
waiting for a connection to be made.


Create a Client
---------------

In your second interpreter, create a client socket and prepare to send a
message::

    >>> import socket
    >>> client_socket = socket.socket(
    ...     socket.AF_INET,
    ...     socket.SOCK_STREAM,
    ...     socket.IPPROTO_IP)

.. container:: incremental

    Before connecting, keep your eye on the server interpreter::

        >>> client_socket.connect(('127.0.0.1', 50000))


Send a Message Client->Server
-----------------------------

As soon as you made the connection above, you should have seen the prompt
return in your server interpreter. The ``accept`` method finally returned a
new connection socket.

.. class:: incremental

When you're ready, type the following in the *client* interpreter. Watch the
server!

.. class:: incremental

::

    >>> client_socket.sendall("Hey, can you hear me?")


Receive and Respond
-------------------

Back in your server interpreter, go ahead and receive the message from your
client::

    >>> conn.recv(32)
    'Hey, can you hear me?'

Send a message back, and then close up your connection::

    >>> conn.sendall("Yes, I hear you.")
    >>> conn.close()


Finish Up
---------

Back in your client interpreter, take a look at the response to your message,
then be sure to close your client socket too::

    >>> client_socket.recv(32)
    'Yes, I hear you.'
    >>> client_socket.close()

And now that we're done, we can close up the server too (back in the server
iterpreter)::

    >>> server_socket.close()


Congratulations!
----------------

.. class:: big-centered

You've run your first client-server interaction


Take it to the Next Level
-------------------------

That's pretty much everything we need to build a simple echo server and
client.

.. class:: incremental

We are now going to move to writing python files.

.. class:: incremental

Quit both interpreters and open a new file in your favorite text editor.  Call
it ``echo_client.py``


The Echo Client - 1
-------------------

.. code-block:: python
    :class: small

    import socket
    import sys

    def client(msg):
        print >> sys.stderr, "sending: %s" % msg

    if __name__ == '__main__':
        if len(sys.argv) != 2:
            usg = '\nusage: python echo_client.py "this is my message"\n'
            print >>sys.stderr, usg
            sys.exit(1)

        msg = sys.argv[1]
        client(msg)

.. class:: incremental

Save that and try it out


Check Your Work
---------------

In your terminal, where you created and saved ``echo_client.py``:

::

    $ python echo_client.py

    usage: python echo_client.py "this is my message"

    $ python echo_client.py "my baloney has a first name"
    sending "my baloney has a first name"


The Echo Client - 2
-------------------

.. code-block:: python
    :class: small

    def client(msg):
        server_address = ('localhost', 10000)
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        print >>sys.stderr, 'connecting to %s port %s' % server_address
        sock.connect(server_address)
        try:
            # Send data
            print >>sys.stderr, 'sending "%s"' % msg
            sock.sendall(msg)
            # Look for the response
            amount_received = 0
            amount_expected = len(msg)
            while amount_received < amount_expected:
                data = sock.recv(16)
                amount_received += len(data)
                print >>sys.stderr, 'received "%s"' % data
        finally:
            print >>sys.stderr, 'closing socket'
            sock.close()


It Takes Two
------------

The client script at this point is no good without a server to receive the
message and send it back.  Let's make that next.

.. class:: incremental

Again, open a new file in your text editor.  Call it `echo_server.py`.


The Echo Server - 1
-------------------

.. code-block:: python
    :class: small

    import socket
    import sys

    def server():
        address = ('127.0.0.1', 10000)
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        print >>sys.stderr, "making a server on %s:%s" % address
        sock.bind(address)
        sock.listen(1)
        try:
            pass #<- what goes here comes in the next slide
        except KeyboardInterrupt:
            sock.close()
            return

    if __name__ == '__main__':
        server()
        sys.exit(0)


The Echo Server - 2
-------------------

.. code-block:: python
    :class: small

    try:
        while True:
            print >>sys.stderr, 'waiting for a connection'
            conn, addr = sock.accept() # blocking
            try:
                print >>sys.stderr, 'connection - %s:%s' % addr
                while True:
                    data = conn.recv(16)
                    print >>sys.stderr, 'received "%s"' % data
                    if data:
                        msg = 'sending data back to client'
                        print >>sys.stderr, msg
                        conn.sendall(data)
                    else:
                        msg = 'no more data from %s:%s' % addr
                        print >>sys.stderr, msg
                        break
            finally:
                conn.close()
    except KeyboardInterrupt:
        # ...


Playing With Your Toy
---------------------

In one terminal, start the server::

    $ python echo_server.py
    making a server on 127.0.0.1:10000
    waiting for a connection

.. class:: incremental

In a second, use the client to send a message:

.. class:: incremental

::

    $ python echo_client.py "I am sending a longer message."


Next Steps
----------

You've now seen the basics of socket-based communication.

.. class:: incremental

In the next session, we'll learn about the protocols that govern these types
of communications.

.. class:: incremental

As an exercise, we'll extend this simple echo server into a basic HTTP
server, and we'll be able to ditch the client and use a web browser instead.


Lunch Time
----------

.. class:: big-centered

We'll see you back here in an hour.  Enjoy!
