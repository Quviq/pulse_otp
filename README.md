pulse_otp
=========

pulse_otp provides slightly modified copies of Erlang OTP
components. These components are meant to be instrumented by the PULSE
tool. This is by no means a complete set of OTP modules, we have only
added modules that we have needed while testing other applications.


Included modules
----------------
Currently the following modules are provided:

 * pulse_gen_server
 * pulse_supervisor
 * pulse_application
 * pulse_gen
 * pulse_proc_lib
 * pulse_application_master
 * pulse_application_controller
 * pulse_application_starter

Installation
------------
In order to compile the modules with PULSE you need <a
href="http://quviq.com">Quviq QuickCheck</a>. There is an *Emakefile*
in the `/ebin` directory that compiles the modules.

Installation is optional, you just need to make sure that the
beam-files can be found by the code loader.


Usage
-----
The usage of these modules are fairly straight forward. One uses the
*pulse_replace_module* funtionality of PULSE. For example:

    -compile({pulse_replace_module, [{gen_server, pulse_gen_server}]}).

Will replace all calls to gen_server by a corresponding call to
pulse_gen_server.

One should note however that using `pulse_application` in parallel
with the normal application modules is a little bit
subtle. Applications usually depend on other applications, for example
many applications require that the `stdlib` application is
running. However, we do normally not want to start a separate
(instrumented) version of `stdlib`. Therefore, `pulse_application`
will also look in the normal application for running
applications. Also, since the application management is normally
started by the `init` process we have made some additions to start
`pulse_application` automatically as soon as it is needed.

Technical notes
---------------
The modules are from Erlang R15B, but in order to compile also on R14
some callback-specs have been commented out.

Other changes to the original modules include:

 * renaming atoms (xxxx to pulse_xxxx)
 * renaminge ETS table names
 * the above mentioned changes in `pulse_application`
