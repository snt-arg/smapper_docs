# SMapper Web App

The [**SMapper Web App**](https://github.com/snt-arg/smapper_app) is a modern, responsive user interface designed to interact with the [SMapper API](./smapper_api.md). It provides a user-friendly way to monitor, control, and manage the SMapper device's services and data recordings.

The application is built with React on top of the Vite tooling ecosystem and uses the Bun runtime for exceptional performance. Component design and layout are handled by the Chakra UI library, which enables rapid and consistent development.

### Key Features

The application is organized into four main pages:

- **Dashboard:** Provides a central overview of the system's status.
- **Visualizer:** Embeds a `rosboard` instance, allowing for real-time visualization of ROS topics such as camera image streams and 3D point clouds.
- **Services:** Allows users to start and stop the various ROS2 nodes and other background services defined in the SMapper API's configuration.
- **Recordings:** Provides an interface to start, stop, and manage ROS bag recordings, listing previous recordings and their metadata.

### Technology Stack

- **Runtime:** [Bun](https://bun.sh/)
- **Bundler:** [Vite](https://vitejs.dev/)
- **Framework:** [React](https://react.dev/)
- **UI Library:** [Chakra UI](https://chakra-ui.com/)
- **Web Server (in Docker):** [Caddy](https://caddyserver.com/)

## Prerequisites

Before you begin, ensure you have **Bun** installed on your system.

- You can find the official installation instructions at [bun.sh/docs/installation](https://bun.sh/docs/installation).

## Configuration

The application is configured using environment variables. Create a `.env` file in the root of the project and populate it with the following variables as needed:

| Variable                                | Description                                                                                            | Required | Default |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------ | :------: | :-----: |
| `VITE_API_BASE_URL`                     | The base URL for the SMapper API endpoint.                                                             | **Yes**  |   \-    |
| `VITE_ROSBOARD_URL`                     | The URL for the `rosboard` instance that will be embedded in the Visualizer page.                      | **Yes**  |   \-    |
| `VITE_SERVICES_POLLING_INTERVAL`        | The interval (in ms) at which the app polls the API for the status of services.                        |    No    | `1000`  |
| `VITE_TOPICS_POLLING_INTERVAL`          | The interval (in ms) at which the app polls the API for the list of available ROS topics.              |    No    | `3000`  |
| `VITE_RECORDING_STATE_POLLING_INTERVAL` | The interval (in ms) at which the app polls the API for the current recording state.                   |    No    | `1000`  |
| `VITE_MOCK_API`                         | Set to `true` to use mocked data instead of calling the real API. Used for UI development and display. |    No    |   \-    |

## Local Development & Usage

!!! note

    All commands should be executed from the root directory of the project.

### Run in Development Mode

Starts the development server with hot-reloading.

```sh
bun run dev
```

### Build for Production

Bundles and optimizes the application for production. The output is placed in the dist/ directory.

```bash
bun run build
```

### Preview Production Build

Starts a local server to preview the production build from the dist/ directory.

```bash
bun run preview
```

## Deployment

Deployment is handled using Docker and Docker Compose. The setup uses a multi-stage `Dockerfile` to create a lean production image served by the high-performance Caddy web server.

The `docker-compose.yml` file defines two services for building the web app:

- `app_dev`: Builds the app for use within a `localhost` environment.

- `app_prod`: Builds the app to be exposed on the local network (e.g., when the device is running a Wi-Fi hotspot).

### Build Process

To build the Docker image, you must pass the `API_URL` and `ROSBOARD_URL` as build arguments.
Bash

```bash
# Example for building the production service
API_URL="[http://10.0.0.1:8000/api/v1](http://10.0.0.1:8000/api/v1)" \
ROSBOARD_URL="[http://10.0.0.1:8888](http://10.0.0.1:8888)" \
docker-compose build app_prod
```

After the build is complete, you can start the service using `docker-compose up app_prod`.
