# Swiggy Clone – Blue-Green Deployment Demo

This project is a React single-page application that recreates the Swiggy landing experience. It is packaged with Docker assets and AWS deployment descriptors so it can serve as the front-end workload in a blue/green deployment pipeline.

## Features
- Responsive navigation bar with location picker, quick links, and Font Awesome icons.
- Horizontally scrollable "Best offers" banner and cuisine spotlight assets.
- Top restaurant chain highlights with offer overlays and ratings.
- Filterable restaurant cards showing delivery offers, cuisines, and service area details.
- Marketing footer with deep links to mobile app stores and quick access to company/support links.

## Tech Stack
- React 18 with `react-scripts`
- React Bootstrap 2 and Bootstrap 5 styling
- Font Awesome Icons via CDN (`public/index.html`)
- Custom component-level CSS modules under `src/Components`

## Getting Started
1. Install dependencies:
   ```bash
   npm install
   ```
2. Start the development server:
   ```bash
   npm start
   ```
   The app runs on `http://localhost:3000` by default.
3. Run the Jest test watcher (configured via `react-scripts`):
   ```bash
   npm test
   ```
4. Produce an optimized production build:
   ```bash
   npm run build
   ```

## Docker Workflow
- Build the image:
  ```bash
  docker build -t swiggy-clone:latest .
  ```
- Run the container locally (maps port 3000):
  ```bash
  docker run --rm -p 3000:3000 swiggy-clone:latest
  ```
  The provided `Dockerfile` installs dependencies, builds the React bundle, and starts the development server. For production you may want to serve the `build/` artifacts with a static web server such as Nginx.

## CI/CD and Blue/Green Deployment
- **`buildspec.yaml`**: Sample AWS CodeBuild configuration that installs prerequisites, runs placeholder steps for scanning/testing, and (when uncommented) builds and pushes a Docker image. It also demonstrates sending build status notifications via Amazon SES.
- **`appspec.yaml`**: AWS CodeDeploy specification targeting an ECS service (`swiggy`) behind a load balancer. CodeDeploy can leverage this to coordinate blue/green switches between task definitions.
- **Blue/Green flow**: Publish a new task definition revision for the `swiggy` service, update the image tag, and let CodeDeploy shift traffic after health checks. The existing `appspec.yaml` binds the load balancer port (`3000`) to the `swiggy` container so green tasks can be verified before cutover.

## Project Structure
```
.
├── Dockerfile
├── appspec.yaml
├── buildspec.yaml
├── package.json
├── public/
└── src/
    ├── App.js
    ├── Components/
    └── Photos/
```
Component-specific styles live next to their JSX counterparts (for example, `src/Components/RestaurentChain.jsx` and `RestaurentChain.css`). Assets used in banners and cards are stored under `src/Photos/` and `public/`.

## Customization Tips
- Update `src/Components` data blocks to reflect live API results or dynamic content if you integrate with a backend.
- Replace placeholder offer/restaurant imagery in `src/Photos` or link to CDN assets that match your deployment region.
- Review the `Dockerfile` if you prefer a multi-stage build that serves the static bundle with Nginx rather than the development server.

## License
This project does not currently declare a license. Add one if you plan to distribute or open-source the code.

