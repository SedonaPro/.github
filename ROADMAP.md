# Community launch roadmap

## In place

- Organization and public community home.
- Separate runtime, Linux editor, Windows editor, ESP32-S3 and ESP32-P4 repositories.
- English project descriptions, contribution guides and issue/PR templates.
- Community Discussions for questions, ideas and showcases.

## First public source preview

- [SedonaPro](https://github.com/SedonaPro/sedonapro): runtime, Pro kits, SDK, simulation and web editor.
- Staged amd64 validation: 126,101 offline VM assertions and 42 web-editor tests passed.
- Independent Studio and ESP source publication remains pending.

## Source preparation

1. Preserve source history and Windows snapshot provenance privately.
2. Review each publication snapshot, dependencies and upstream notices.
3. Document the exact SDK/kit/firmware combinations that work together.
4. Verify a clean-clone build for each supported target.
5. Write project-specific English quick starts and hardware support tables.
6. Add reviewed screenshots, release checksums and reproducible release inputs.

## Public launch

- Open each project only after its publication review.
- Announce verified releases with limitations and test scope.
- Offer small documented contribution tasks.
- Publish tested examples and board reports from contributors.

No release dates or platform guarantees are implied by this roadmap.

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
