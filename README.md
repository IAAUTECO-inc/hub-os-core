
**🛡️ FORT GRAYMOR (Hub OS): The Secure IoT/Edge Gateway**

FORT GRAYMOR is the critical infrastructure component within the **WINTERHOLD ecosystem**. It acts as the central, hardened IoT/Edge Hub, securely bridging raw perception data from local sensors (cameras, microphones, etc.) to the high-level Cognitive Backend (MASAQ, Skald, Delphine).

Built on a minimalist FreeBSD operating system, Fort Graymor is the cornerstone of our Zero Trust Architecture (ZTA), ensuring that all data entering the system is validated, sanitized, and compliant with critical security standards before processing.

**⚙️ Architecture and Role**
Fort Graymor sits at the heart of the system, acting as a highly secure membrane between isolated network segments.

**Core Architecture Flow**
The system operates based on strict network segregation and protocol handling:

Jetson-Net (VLAN IoT): Isolated network for all sensor nodes (Jetson devices, cameras, microphones).

Fort Graymor (Hub OS): Intercepts all traffic from Jetson-Net.

API Gateway (Go): The internal service layer that handles routing to the specific backend modules (Mnemosyne, Vesta, etc.) and databases (PostgreSQL/pgvector).

**Primary Functions**
Event Ingestion (/iot/events): Securely receives encrypted JSON event payloads from the decentralized Jetson sensor nodes.

Consensus and Aggregation: Performs multi-node validation (e.g., verifying an event with inputs from 2 cameras/room) to filter noise and increase certainty before routing to the AI layer.

Zero Trust Gateway: Enforces mTLS (mutual TLS) authentication, requiring valid certificates from every connecting Jetson node, coupled with stringent rate limiting and firewall filtering.

Normalization: Translates raw, heterogeneous sensor events into a standardized, clean format required by the backend cognitive modules.

Secure Routing: Routes validated and normalized events to the appropriate internal destinations, including the API Gateway, PostgreSQL/pgvector for historical indexing, and Llamafile inference engines.

**🔒 Security and Compliance (NIS2 / ISO)**
Security is paramount and is enforced through architectural design, leveraging the inherent robustness of the FreeBSD base.

Network Isolation: Dedicated, isolated VLAN IoT (Jetson-Net) ensures the sensor network cannot directly access the core backend services.

Hardened OS: Utilizes FreeBSD Jails/Containers to isolate critical services within the Hub OS, minimizing potential lateral movement in case of a breach.

Immutable Logs: System logs are designed to be immutable, ensuring a complete and auditable history required for regulatory compliance.

Software Bill of Materials (SBOM): SBOM generation is integrated into the deployment process via Kestra, providing full transparency and vulnerability scanning for all dependencies.

**💻 Exposed APIs (Zero Trust Interaction)**
Fort Graymor exposes a minimal, tightly controlled set of APIs for interaction with the sensor and management layers. All endpoints require valid mTLS certificates.

POST /iot/events: Endpoint for receiving real-time, encrypted event data from the Jetson sensor nodes.

GET /iot/config: Endpoint for sensor nodes to securely fetch current operational policies and spatial zone definitions.

POST /iot/health: Endpoint for nodes to report their current operational status and health metrics.

**🛠️ Technical Stack**
Base OS: FreeBSD (Selected for security, stability, and minimal attack surface).

Networking: Dedicated VLAN and advanced firewall configurations.

Security Layer: mTLS, Zero Trust principles, Certificate Authority management.

Orchestration: Kestra is used for automated deployment, monitoring, and maintaining the SBOM integrity of the Hub OS.

Fort Graymor is the single point of defense and the validator of all perception data in the WINTERHOLD ecosystem. All development must strictly adhere to the principles of separation of concerns and Zero Trust.

---
FreeBSD Source:
---------------
This is the top level of the FreeBSD source directory.

FreeBSD is an operating system used to power modern servers, desktops, and embedded platforms.
A large community has continually developed it for more than thirty years.
Its advanced networking, security, and storage features have made FreeBSD the platform of choice for many of the busiest web sites and most pervasive embedded networking and storage devices.

For copyright information, please see [the file COPYRIGHT](COPYRIGHT) in this directory.
Additional copyright information also exists for some sources in this tree - please see the specific source directories for more information.

The Makefile in this directory supports a number of targets for building components (or all) of the FreeBSD source tree.
See build(7), config(8), [FreeBSD handbook on building userland](https://docs.freebsd.org/en/books/handbook/cutting-edge/#makeworld), and [Handbook for kernels](https://docs.freebsd.org/en/books/handbook/kernelconfig/) for more information, including setting make(1) variables.

For information on the CPU architectures and platforms supported by FreeBSD, see the [FreeBSD
website's Platforms page](https://www.freebsd.org/platforms/).

For official FreeBSD bootable images, see the [release page](https://download.freebsd.org/ftp/releases/ISO-IMAGES/).

Source Roadmap:
---------------
| Directory | Description |
| --------- | ----------- |
| bin | System/user commands. |
| cddl | Various commands and libraries under the Common Development and Distribution License. |
| contrib | Packages contributed by 3rd parties. |
| crypto | Cryptography stuff (see [crypto/README](crypto/README)). |
| etc | Template files for /etc. |
| gnu | Commands and libraries under the GNU General Public License (GPL) or Lesser General Public License (LGPL). Please see [gnu/COPYING](gnu/COPYING) and [gnu/COPYING.LIB](gnu/COPYING.LIB) for more information. |
| include | System include files. |
| kerberos5 | Kerberos5 (Heimdal) package. |
| lib | System libraries. |
| libexec | System daemons. |
| release | Release building Makefile & associated tools. |
| rescue | Build system for statically linked /rescue utilities. |
| sbin | System commands. |
| secure | Cryptographic libraries and commands. |
| share | Shared resources. |
| stand | Boot loader sources. |
| sys | Kernel sources (see [sys/README.md](sys/README.md)). |
| targets | Support for experimental `DIRDEPS_BUILD` |
| tests | Regression tests which can be run by Kyua.  See [tests/README](tests/README) for additional information. |
| tools | Utilities for regression testing and miscellaneous tasks. |
| usr.bin | User commands. |
| usr.sbin | System administration commands. |

For information on synchronizing your source tree with one or more of the FreeBSD Project's development branches, please see [FreeBSD Handbook](https://docs.freebsd.org/en/books/handbook/cutting-edge/#current-stable).
