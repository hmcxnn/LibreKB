# LibreKB

LibreKB: Knowledge Base Web App - Free, Open Source, Self Hosted, PHP/MySQL.

Check out the official website at [LibreKB.com](https://librekb.com/) for more information.

### Features

- 100% free and open source.
- Runs on pretty much any server or hosting account.
- Installs in minutes.
- Easy to customize branding to match your project or business.
- Manage users with predefined user groups.
- Easily edit articles with TinyMCE.
- Password resets via email.
- Responsive design based on Bootstrap.

### Need Help?

Docs: [docs.LibreKB.com](https://docs.librekb.com/)

Forum: [GitHub Discussions for LibreKB](https://github.com/michaelstaake/LibreKB/discussions)

### Update Checks and Your Privacy

If you have updateCheck set to 'yes' in the config file, which is the default setting, your LibreKB install will reach out to my servers every time you log in to your LibreKB admin area. The update checker is hosted on my server and may log the IP address of your server, the version of LibreKB you're currently running, and the timestamp of when the update check was requested. No other data is submitted or collected. I do not intend for this data to be shared or sold, but I reserve the right to use this data however I determine is best, including but not limited to posting anonymized stats on usage. If you do not wish to use this feature, simply set updateCheck to no in the config file, and no data will ever be sent to my servers.

### Docker Setup

For instructions on running this project with Docker, check out [DOCKER.md](DOCKER.md).

#### GitHub Container Registry

This project includes GitHub Actions that automatically build and publish Docker images to GitHub Container Registry. For quick deployment using pre-built images, see [GITHUB_CONTAINER.md](GITHUB_CONTAINER.md).

**Available Images:**
- `ghcr.io/[your-username]/librekb:latest` - Latest build from main branch
- `ghcr.io/[your-username]/librekb:v*` - Tagged releases

**Quick Start with Pre-built Image:**
```bash
# Pull the latest image
docker pull ghcr.io/[your-username]/librekb:latest

# Use with docker-compose (see GITHUB_CONTAINER.md for full setup)
docker-compose up -d
```
