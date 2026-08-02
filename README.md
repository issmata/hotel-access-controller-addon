# Hotel Access Controller Home Assistant Add-on

Public Home Assistant add-on repository for the Hotel Access Controller development agent.

## Installation

1. In Home Assistant, open **Settings > Apps > App store**.
2. Open the repository menu and add:
   `https://github.com/issmata/hotel-access-controller-addon`
3. Refresh the app store and install **Hotel Access Controller Development**.
4. Configure the SaaS URL, controller ID, and controller token.
5. Start the app and enable **Start on boot** and **Watchdog**.

The controller application source remains in its private development repository. This repository contains only the Home Assistant add-on catalog metadata and documentation.

Release `0.1.0-dev.2` packages encrypted offline scheduling and durable reconciliation. Upgrade only after the matching Controller image tag has been published by the runtime repository workflow.
