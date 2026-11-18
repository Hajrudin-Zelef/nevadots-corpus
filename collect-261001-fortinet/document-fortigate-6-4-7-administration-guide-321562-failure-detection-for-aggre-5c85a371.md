# Failure detection for aggregate and redundant interfaces

When an aggregate or redundant interface goes down, the corresponding fail-alert interface changes to down. When an aggregate or redundant interface comes up, the corresponding fail-alert interface changes to up.


Fail-detect for aggregate and redundant interfaces can be configured using the CLI.

###### To configure an aggregate interface so that port3 goes down with it:

config system interface

edit "agg1"

set vdom "root"

set fail-detect enable

set fail-alert-method link-down

set fail-alert-interfaces "port3"

set type aggregate

set member "port1" "port2"

next

end

###### To configure a redundant interface so that port4 goes down with it:

config system interface

edit "red1"

set vdom "root"

set fail-detect enable

set fail-alert-method link-down

set fail-alert-interfaces "port4"

set type redundant

set member "port1" "port2"

next

end