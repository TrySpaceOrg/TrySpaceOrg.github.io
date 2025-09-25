# Getting Started with TrySpace Lab

A short quick-start to get a up and running with TrySpace Lab.

## Walkthrough

Note that the speed at which you can install is subject to your internet connection and the performance of your computer.
Gigabytes of data are required to be downloaded during this process.

* Ensure Docker, Docker Compose (v2+), Git, and Make are installed (see [Installation](installation.md)).
* Clone the repository to your computer:
```bash
git clone https://github.com/TrySpaceOrg/tryspace-lab.git
cd tryspace-lab
```

<img src="/assets/manual/getting-started/Screenshot 2025-09-10 081103.png" alt="Screenshot 2025-09-10 081103" class="center" />

* Download and initialize the submodules: `git submodule update --init --recursive`

<img src="/assets/manual/getting-started/Screenshot 2025-09-10 081134.png" alt="Screenshot 2025-09-10 081134" class="center" />

* Prepare environment and build: `make`

<img src="/assets/manual/getting-started/Screenshot 2025-09-10 081427.png" alt="Screenshot 2025-09-10 081427" class="center" />

* Start the lab: `make start`

<img src="/assets/manual/getting-started/Screenshot 2025-09-10 081445.png" alt="Screenshot 2025-09-10 081445" class="center" />

The various services will take a few seconds to stabilize and flight software to finish it's initialization.

<img src="/assets/manual/getting-started/Screenshot 2025-09-10 081536.png" alt="Screenshot 2025-09-10 081536" class="center" />

* Access GSW (YAMCS): http://localhost:8090

<img src="/assets/manual/getting-started/Screenshot 2025-09-10 081541.png" alt="Screenshot 2025-09-10 081541" class="center" />

* You'll be able to send commands, view the current links, etc. in YAMCS so poke around!

<img src="/assets/manual/getting-started/Screenshot 2025-09-10 081625.png" alt="Screenshot 2025-09-10 081625" class="center" />

* Attach to consoles in a new tab: `docker attach tryspace-server`

<img src="/assets/manual/getting-started/Screenshot 2025-09-10 081740.png" alt="Screenshot 2025-09-10 081740" class="center" />

* Stop (preserves data): CTRL+C in primary console

<img src="/assets/manual/getting-started/Screenshot 2025-09-10 081910.png" alt="Screenshot 2025-09-10 081910" class="center" />

* If you did CTRL+C twice to stop quickly, you may need to `make stop` prior to running again.

<img src="/assets/manual/getting-started/Screenshot 2025-09-10 081920.png" alt="Screenshot 2025-09-10 081920" class="center" />

* Clean (removes data): `make clean`

<img src="/assets/manual/getting-started/Screenshot 2025-09-10 081942.png" alt="Screenshot 2025-09-10 081942" class="center" />

* Looking to reclaim some data? `make clean-cache`

<img src="/assets/manual/getting-started/Screenshot 2025-09-10 082038.png" alt="Screenshot 2025-09-10 082038" class="center" />

* Want to uninstall? `make uninstall`

<img src="/assets/manual/getting-started/Screenshot 2025-09-10 082050.png" alt="Screenshot 2025-09-10 082050" class="center" />

Have trouble with any of the above?
Checkout the [Frequently Asked Questions](faq.md).
