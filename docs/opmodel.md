# Prol Operational Model

The lifecycle of a Prol instance is determined entirely by its masterscript, a
PR script fed to it at inception. PR is a domain-specific language created for
and used only by Prol. It is imperative by nature and is designed to be
'expressively restrictive', offering a small feature-set tailored with elegance
in mind. The Prol application offers a tiny set of command-line options, mostly
used to introduce the masterscript to the instance, which is kept small in order
to defer as much execution to the user's own masterscript as possible. The only
command-line switch that must be known for operation is *-ms <path>* which
defines the path to the masterscript; if omitted, the instance attempts to
process the masterscript from standard input. Once the masterscript has been
obtained, it is executed as any interpreted language script, which each command
translating to one or more **actions** taken by the instance.

PR is strictly imperative, in order to enforce the idea that the user is solely
responsible for specifying each and every action the instance takes. Despite
this, instance configuration is one domain where imperative implementations (see
CMake's set()) are extremely ugly, and thus ought be implemented in a
declarative manner. To fix this issue while keeping PR imperative, a second DSL
exists, contained entirely within its file format: that of the rulefile. A
rulefile is essentially a Prol configuration file written in simple key=value
pairs, where each key is an option configurable within a Prol instance, and its
corresponding value is a valid setting of said key. Rulefiles are loaded with
the PR command *RULE <path>*, which loads the rules within from the rulefile at
the path specified. Rule loading is entirely linear in time: if a rule is
defined in a loaded rulefile, it may be effortlessly overwritten by loading a
different rulefile defining the same rule at a later point in script execution.

A typical masterscript might perform the following functions sequentially:
1. Configure instance settings, such as multithreading capabilities and
   firewalled IP addresses, by loading the rulefiles containing them.
2. Load an action module for later use, in order to associate the module's PR
   commands and Rule rules with the instance early.
3. Configure instance module settings by loading the rulesfiles containing them.
4. Define a list of potential peer IP addresses, and then loop through them,
   attempting to connect to each one in turn.
5. Establish synchronicity with connected peers.
6. Execute module commands to perform the instance's primary objective.
7. Loop back to Step 5.

Another point to note is that the 'masterscript' is merely an abstraction for
one or more PR scripts; the abstraction exists to conceptualize all scripted
information as belonging to a single central script. What this means is that
nothing in the specification exists exploring or prohibiting a separation of
masterscript logic; using the *PR <path>* command, it is possible to load an
external PR script from within a loaded one, thus chaining scripts together.
This allows for the separation of masterscript logic amongst modular components,
which can further enhance masterscript design.
