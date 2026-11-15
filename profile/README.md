# SedonaPro

### Build control logic. See it run. Bring it to hardware.

SedonaPro is a community of Sedona enthusiasts building an open, practical
platform for automation. We bring together control engineers, software developers,
electronics makers and curious learners to develop tools, share experience and
turn ideas into working control systems.

Whether you are exploring your first function-block diagram or designing your
own controller, there is room to experiment, ask questions and contribute.

Our work spans the whole path from an idea to a running controller: a portable
runtime and reusable control kits, desktop tools for Linux and Windows, a browser-based
web editor, firmware, simulation and practical examples for building automation.
Sedona can be ported to many controller platforms with suitable resources and a
platform adaptation layer; Linux, ESP32-S3 and ESP32-P4 are our current development targets.
You can explore an algorithm on a computer, watch signals change, develop your
own components and work toward an application on a supported board.

## Visual engineering in action

![Sedona Studio Windows: live block diagram, signal panel and trends](https://raw.githubusercontent.com/SedonaPro/.github/main/profile/images/windows-live-engineering.png)

**Sedona Studio Windows 0.7.0 — live engineering with an English interface.**
Two ramp generators feed an adder and a formula component. The canvas shows
typed ports, connections and live values; the right-hand panel displays a watched
output, and the trend view below compares selected signals over time. The tree
on the left provides access to the application's folders and components. This
user-supplied screenshot shows a connection to a local runtime; it illustrates
the Windows edition, whose features can differ from the Linux edition.

## Five projects, clear responsibilities

| Project | What you will find | Who it is for |
| --- | --- | --- |
| **[SedonaPro](https://github.com/SedonaPro/sedonapro)** | VM, compiler, SDK tooling, reusable `spro*` kits, profiles, simulation and the current web editor | Runtime developers, control engineers and kit authors |
| **[Sedona Studio for Linux](https://github.com/SedonaPro/sedona-studio-linux)** | Desktop diagram engineering, local simulation, kit tooling and optional operator screens, alarms and value history | Linux users developing and exploring applications |
| **[Sedona Studio for Windows](https://github.com/SedonaPro/sedona-studio-windows)** | Desktop diagram engineering, live diagnostics, local simulation and a native C kit workshop | Windows users who want an integrated engineering workflow |
| **[Sedona for ESP32-S3](https://github.com/SedonaPro/sedona-esp32-s3)** | ESP-IDF integration, platform services, SOX connectivity and simulator tooling | Embedded developers working with ESP32-S3 |
| **[Sedona for ESP32-P4](https://github.com/SedonaPro/sedona-esp32-p4)** | Ethernet controller firmware, I/O services, Modbus, MQTT and update tooling | Firmware developers and controller-board designers |

Linux and Windows editors have separate repositories and release tracks.
ESP32-S3 and ESP32-P4 firmware are also independent projects. Shared concepts
and file formats connect them; compatibility depends on exact SDK, kit and
firmware versions and will be documented per release.

## Modernizing the runtime

A major part of this work is bringing the runtime to **64-bit Linux (amd64)**
and fixing issues exposed by the port. Changes include reference-size-aware
field layout, null-pointer initialization, VM stack handling across native
calls, UDP structure layout and pointer-safe native interfaces, including
Modbus handles. The build tooling has also been updated for Python 3.

SedonaPro is therefore more than a collection of board ports: runtime
correctness, compiler compatibility and reliable native integration are central
to the project. These changes are backed by existing runtime and integration
tests; publication builds will document their exact verification results.

## Protocol and communication improvements

We have also improved the communication stack and the way engineering tools
use it, with a focus on reliable transfers, live sessions and portable clients.

| Area | Improvements in our development lines |
| --- | --- |
| SOX file transfers | Fixed a race between the receiver and caller threads in the Java client's chunk queue, which could corrupt the queue, lose chunks and cause transfer timeouts |
| Read recovery in Linux Studio | Releases an abandoned file transfer before retrying a failed read, with at most three attempts; device writes are not automatically retried |
| Larger application schemas | Our Studio and web clients request a 1024-byte DASP packet limit for the Pro profile; older clients must be checked for compatibility |
| Device-provided resources | SOX access to kit manifests and widget resources lets supporting editors obtain the metadata and views associated with the installed kit builds |
| Web live sessions | Fixed a watch-renewal race during configurator rebuilds: components removed by a rebuild are excluded from the watch list instead of ending the live session |
| Operator-screen transfer | The web gateway writes the screen package as a separate device file and checks its bytes by reading it back; concurrent file transfer and device mutations are coordinated |
| MQTT | A non-blocking MQTT 3.1.1 client polled by the VM, with QoS 0/1, retained messages, last will, keep-alive, reconnect backoff and subscription restoration; shared implementation for Linux, simulation and ESP-IDF |

These include protocol-client fixes and editor/gateway behaviour, not a claim
that every feature changes the SOX/DASP wire format. Support differs by editor,
SDK and firmware version. Existing source tests cover relevant scenarios;
release-specific results will accompany verified builds.

## Web editor and an emerging SCADA environment

The **web editor** brings visual Sedona engineering to the browser: navigate
applications, edit function-block diagrams and properties, inspect live values,
and create graphical views through a Java server and React interface. Its source
currently lives in `sedonapro/editor/`.

Beyond diagram engineering, we have built much of an operator environment:
graphical screens, reusable equipment symbols and components, live bindings,
operator commands and trends. Linux Studio also has an optional SCADA module
with alarm handling, acknowledgement, an event journal and stored value history.
Features differ between the web, Linux and Windows editions.

**Almost a SCADA:** a substantial foundation is already implemented, while
standalone station operation without the editor and user/role management remain
unfinished. We describe the available pieces explicitly rather than presenting
this as a complete SCADA product.

## Control kit library and system configurators

The Pro kit library is a central part of SedonaPro. It combines individual
control components with configurable assemblies that build a system's internal
blocks, I/O points and links from selected equipment options.

| Configurator | Kit | Configuration and generated logic |
| --- | --- | --- |
| Air handling unit | `sproAhu` | Supply or supply/exhaust; water or electric heating; cooling, heat recovery, fan control, sensors and interlocks |
| Pump station | `sproPumpSt` | 1–4 pumps; on/off, common or individual VFDs; PID, staging, duty/standby rotation, dry-running protection and optional feedback |
| Heating substation | `sproHeatSt` | Up to two heating circuits, outdoor-reset curves, single or twin pumps, optional domestic hot water, circulation, make-up and common alarm |

**How it works:** add the configurator, select the equipment and options, then
invoke **Build**. The assembly creates the required components and connections.
Where supported, its kit-supplied widget offers setup and operation views.
Compatible SDK, kit manifests and editor support are required; support must be
verified separately for each editor edition.

A rebuild retains existing blocks of the same type and their settings, adds
required parts and removes obsolete generated parts. Custom logic belongs next
to the generated assembly. Automatic channel assignment reports the I/O list
and points left without a board channel; it does not replace electrical design
or commissioning.

The underlying library includes signal conditioning, PID and sequencing,
equipment control, alarms, schedules, HVAC, heating, energy functions and plant
simulation. Domain packs (`control`, `pumps`, `hvac`, `heat`, `energy`) select
related kits for a build. The library currently lives in the **sedonapro** source
project; it is distinct from graphical screen-symbol libraries.

## What you can explore

- **Control logic:** connected function blocks, formulas, schedules, signal
  processing and reusable control components.
- **Building automation:** heating, ventilation, pumps, equipment sequencing,
  alarms and simulated plant models in the Pro kit collection.
- **Visual tools:** application navigation, component properties, live values,
  trends and project files you can keep under version control.
- **Embedded development:** board support, platform services, communication
  protocols and the path from a host-side experiment to firmware.
- **Your own kits:** small reusable components, source examples and native
  extensions where the target supports them.

These are areas covered by the projects collectively. Features and hardware
support vary by edition and target; a screenshot is not a compatibility matrix.

## Find your starting point

**Learning Sedona?** Start with a small simulator example: a signal source,
a calculation and a trend. Ask questions and share what you learned.

**Already an automation engineer?** Bring a reusable control pattern, a clear
application example or feedback on the engineering workflow.

**Building boards or firmware?** Tell us the exact board, chip and toolchain.
Help turn board-specific experiments into documented, reproducible support.

**Writing tools or kits?** Discuss the interface or format first, then contribute
a focused change with an example others can build and test.

## Where we want to take SedonaPro

Our ambition is a broader automation ecosystem: more controller platforms,
more ways to connect equipment, a growing control and simulation kit library,
and **SedonaPro SCADA** as a dedicated operator environment.

These are development directions, not announcements of completed drivers or
committed delivery dates. Scope and priorities will evolve with contributors,
available hardware and real application needs. The protocols and platforms listed
below are examples, not a closed list: further integrations and kit domains are
welcome as the community grows.

| Direction | Planned work |
| --- | --- |
| More controllers | Port the runtime and HAL to additional embedded and industrial platforms; document board support, toolchains and tested I/O |
| Building-automation networks | Explore BACnet/IP, BACnet MS/TP, N2 Open, KNX and other protocols, including the required transports, hardware interfaces and interoperability tests |
| REST and other drivers | Develop HTTP/REST integrations, gateways and other protocol and equipment drivers around clear data models and documented interfaces |
| SedonaPro SCADA | Grow the existing screens, live bindings, commands, alarms, trends and history into a dedicated operator environment, including standalone station operation and user/role management |
| More control kits | Expand reusable libraries for HVAC, heating, pumps, energy, equipment, sequencing and system configurators |
| Object and plant models | Add models of physical processes, equipment, sensors and actuators, with configurable dynamics, disturbances and fault scenarios |
| Offline engineering | Develop and debug control logic without physical equipment: local simulation, accelerated model time, repeatable scenarios, regression checks and clear diagnostics |
| Shared engineering resources | Grow example applications, graphical libraries, documentation, translations and reusable test cases across tools |

A useful contribution can be a driver prototype, a board port, a validated
plant model or a well-documented offline test scenario. Bring ideas to
[community Discussions](https://github.com/SedonaPro/.github/discussions/categories/ideas)
so we can agree on interfaces and avoid duplicating work.

## Join the community

- [Q&A](https://github.com/SedonaPro/.github/discussions/categories/q-a) — ask about concepts, tools and workflows.
- [Show and tell](https://github.com/SedonaPro/.github/discussions/categories/show-and-tell) — share diagrams, boards and experiments.
- [Ideas](https://github.com/SedonaPro/.github/discussions/categories/ideas) — discuss improvements before a large change.
- [Announcements](https://github.com/SedonaPro/.github/discussions/categories/announcements) — follow project news and release announcements.
- [Introduce yourself](https://github.com/SedonaPro/.github/discussions/1) — tell us what you are building.

Documentation, minimal reproductions, example diagrams, board testing and
translations are valuable contributions alongside code. English is the reference
language. Questions in Russian are welcome; an English summary helps more people
participate. Public screenshots use English interface text and demo labels, with
an explanatory caption directly below the image.

## Current status

The **[SedonaPro source preview is public](https://github.com/SedonaPro/sedonapro)**:
portable runtime, Pro kits, system configurators, SDK tooling, simulation and the
web editor. Start with its [quick start](https://github.com/SedonaPro/sedonapro/blob/main/docs/quickstart_EN.md)
and [validation report](https://github.com/SedonaPro/sedonapro/blob/main/docs/source-preview-validation.md).
The staged amd64 build passed runtime smoke checks, 126,101 offline VM assertions
and 42 web-editor tests; see the report for exclusions.

All five projects now have public source previews. The Linux and Windows
editors passed source-build and core tests; Linux also passed headless GUI
checks. The S3 and P4 Sedona-side image builds passed using their included
runtime sources. Each repository documents its verification scope.

Project-authored code in the independent editors and ports uses Apache-2.0;
framework and third-party components retain their own licences. Historical
development commits remain private. Downloadable application and firmware
releases will be announced separately after their own checks.

See the [launch roadmap](https://github.com/SedonaPro/.github/blob/main/ROADMAP.md).
There is no release date or universal hardware-support promise at this stage.

## Engineering and collaboration

We value small reproducible examples, explicit versions and useful test evidence.
Simulator checks, on-board software checks and electrical I/O acceptance are
reported separately. Contributions go through focused branches and review;
upstream attribution and third-party licences stay with their components.

Read [Contributing](https://github.com/SedonaPro/.github/blob/main/CONTRIBUTING.md),
[Support](https://github.com/SedonaPro/.github/blob/main/SUPPORT.md) and the
[community guidelines](https://github.com/SedonaPro/.github/blob/main/CODE_OF_CONDUCT.md).

SedonaPro is an independent community project, not affiliated with or endorsed
by Tridium. Component licences and upstream notices are documented in the
individual source projects.
