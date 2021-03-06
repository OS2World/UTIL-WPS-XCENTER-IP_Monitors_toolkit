IP Monitors toolkit
===================

This archive is a collection of scripts you can use to create TCP/IP
monitors.

They are intended for use with XWorkplace V0.9.12 or higher and the
XWorkplace Widget Library V0.5.2 or higher.

It consists of a REXX DLL and a couple of sample REXX scripts for use
in a gauge widget.


Archive content
===============

  ipmon14.wgt  - a sample multi-interface monitor
  readme       - this file
  rxtcpmon.dll - the helper DLL
  src.zip      - the DLL source file
  unimon.wgt   - a single interface monitor


Installation
============

Put the RXTCPMON.DLL file somewhere along your LIBPATH.

Then, add one or more REXX gauge widgets to an XCenter, and then
set the gauge widgets to your liking.

Two sample gauge scripts are provided:

  ipmon14.wgt - this script monitors up to 4 interfaces.  For each
                interface you can tailor the background color.

                For each interface it displays the interface index,
                and the amount of inbound and outbound traffic in
                the last second.

                Using a fixed-width font is recommended for this
                widget (to do that, simply open the font palette and
                drag one over the widget you want to change).

                A refresh rate of 1000 or 2000 ms is appropriate
                in most cases.

  unimon.wgt  - this script monitor just one interface.  It displays
                a moving gauge representing the traffic in the last
                second.  The tooltip shows the interface name, the
                total inbound and outbound traffics, and the
                maximum throughput recorded.

                The default monitored interface is the one with
                index 10 (here, the PPP0 interface).  You can
                change it at the top of the script.

                [The threshold also defined at the top of the script
                 is the minimum percentage a non-null traffic will
                 be represented with.  It is sometime handy to
                 exaggerate small traffics so that they are more
                 noticeable.  If you do not like it, define the
                 threshold to 0.]

                A refresh rate of 1000 or 2000 ms is appropriate in
                most cases.


Writing your own monitors
==========================

You are very welcome to create your own TCPIP monitors.  The
included scripts can do a starting point.

The provided DLL, RXTCPMON, defines some REXX functions you
may find useful while designing your monitor:

  DosQuerySysInfo

    This function takes one parameter, a number, and return a
    number.  It interfaces with the DosQuerySysInfo OS/2 function.

    For the purpose of TCPIP monitors, DosQuerySysInfo(14) is
    probably the most useful data.  It returns the number
    of millisecond elapsed since IPL.  Handy for computing
    the average traffic per seconds and such.

    Usage: millis = DosQuerySysInfo(14)

    Refer to the OS/2 Developer Toolkit for more information on
    the various things you can query.

  TCPQueryRecCount

    Returns the received byte count, as returned by the SIOSTATTCP
    socket ioctl.

    Usage: rec = TCPQueryRecCount()

  TCPQuerySndCount

    Returns the sent byte count, as returned by the SIOSTATTCP
    socket ioctl.

    Usage: sent = TCPQuerySndCount()

  TCPQueryStats

    Returns the received and sent byte count as returned by the
    SIOSTATTCP socket ioctl.

    Usage: parse value TCPQueryStats() with received sent .

    [Please note the trailing '.'.  Newer release of the DLL may
     return more values.]

  TCPQueryAvailableInterfaces

    Returns a list of available interfaces, as returned by the
    SIOSTATAT socket ioctl.

    Usage: list = TCPQueryAvailableInterfaces()
           do i = 1 for words(list)
             say 'interface #'word(list,i) 'is available'
           end

  TCPQueryInterface

    Returns the available data for the specified interface, as
    returned by the SIOSTATIF socked ioctl.

    It returns a a space-separated list of interface data.  Namely,
    and in this order:

      - index (same as parameter if valid)
      - type
      - mtu size
      - physical address (12 hex digits)
      - OperStatus
      - Speed
      - LastChange
      - InOctets
      - OutOctets
      - OutDiscards
      - InDiscards
      - InErrors
      - OutErrors
      - InUnknownProtos
      - InUcastPkts
      - OutUcastPkts
      - InNUcastPkts
      - OutNUcastPkts
      - description (up to the end of the returned string)

    All elements are numbers, except for description, which
    can be any string (including the null string).

  TCPLoadFuncs

    Loads the functions exported by the RXTCPMON DLL.

    Usage:  call TCPLoadFuncs

  TCPDropFuncs

    Unloads the functions exported by the DLL.  Please note that in
    most cases it is _not_ recommended to call this function, as this
    unloads the functions for _all_ the system, and hence may cause
    problems if some other scripts were using them.

    Usage:  call TCPDropFuncs


Miscellaneous
=============

This IP Monitors toolkit has been developed by Cliff Brady and
Martin Lafaix.  You are free to use for any purpose.

Enjoy.
