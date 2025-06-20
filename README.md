# AYON Development Workspace

A unified VS Code workspace for developing the AYON media production pipeline platform. This workspace orchestrates multiple repositories and provides a seamless development environment for the entire AYON ecosystem.

## 🏗️ Project Structure

This workspace includes the following AYON components:

- **ayon-dev-workspace** - This repository (workspace configuration)
- **ayon-docker** - AYON server infrastructure and Docker configurations
- **kitsu-docker** - Kitsu production tracking server setup
- **ynput-ayon-core** - Core AYON platform libraries and APIs
- **ayon-launcher** - Desktop application launcher for AYON
- **ynput-ayon-maya** - Maya integration addon (example)
- Additional addons can be added as needed (Blender, Houdini, etc.)

## 📋 Prerequisites

Before setting up this workspace, ensure you have the following installed:

### Required Software

1. **Git** (version 2.30+)

   ```bash
   git --version
   ```

2. **Docker & Docker Compose** (latest stable version)

   ```bash
   docker --version
   docker compose version
   ```

3. **Python** (version 3.9+)

   ```bash
   python3 --version
   ```

4. **Poetry** (Python dependency management)

   ```bash
   curl -sSL https://install.python-poetry.org | python3 -
   ```

5. **Node.js & npm** (version 18+ for frontend components)

   ```bash
   node --version
   npm --version
   ```

6. **VS Code** (latest stable version)
   - Download from [code.visualstudio.com](https://code.visualstudio.com/)

## 🚀 Quick Setup

### 1. Create Development Directory

```bash
# Create a dedicated directory for AYON development
mkdir -p ~/Dev/ayon-dev
cd ~/Dev/ayon-dev
```

### 2. Clone This Workspace Repository

```bash
git clone https://github.com/your-org/ayon-dev-workspace.git
cd ayon-dev-workspace
```

### 3. Clone Required Repositories

Clone all AYON repositories in the parent directory:

```bash
# Navigate to the parent directory
cd ..

# Clone core repositories
git clone https://github.com/ynput/ayon-docker.git
git clone https://github.com/cgwire/kitsu-docker.git
git clone https://github.com/ynput/ayon-core.git ynput-ayon-core
git clone https://github.com/ynput/ayon-launcher.git

# Clone addon repositories (add more as needed)
git clone https://github.com/ynput/ayon-maya.git ynput-ayon-maya
# git clone https://github.com/ynput/ayon-blender.git ynput-ayon-blender
# git clone https://github.com/ynput/ayon-houdini.git ynput-ayon-houdini
```

Your directory structure should now look like:

```
~/Dev/ayon-dev/
├── ayon-dev-workspace/
├── ayon-docker/
├── kitsu-docker/
├── ynput-ayon-core/
├── ayon-launcher/
└── ynput-ayon-maya/
```

### 4. Open VS Code Workspace

```bash
cd ayon-dev-workspace
code .vscode/ayon-dev.code-workspace
```

### 5. Install Recommended Extensions

VS Code will prompt you to install recommended extensions. Click "Install All" or install them individually:

- **Python Extension Pack** - Python development support
- **Ruff** - Fast Python linter and formatter
- **Docker** - Container management
- **GitLens** - Enhanced Git capabilities
- **VS Code Icons** - Better file/folder icons
- **Error Lens** - Inline error highlighting

## 🔧 Environment Setup

### Python Environment Setup

Each Python project should have its own virtual environment:

```bash
# For ayon-core
cd ../ynput-ayon-core
python3 -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
pip install --upgrade pip
poetry install

# For ayon-launcher
cd ../ayon-launcher
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
poetry install

# For each addon (e.g., ayon-maya)
cd ../ynput-ayon-maya
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
poetry install
```

### Docker Environment Setup

Set up the server infrastructure:

```bash
# Start AYON server
cd ../ayon-docker
docker compose up -d

# Start Kitsu server (optional, for production tracking)
cd ../kitsu-docker
docker compose up -d
```

## 🎯 Development Workflow

### Running Tasks

Use VS Code's integrated task runner (`Ctrl+Shift+P` → "Tasks: Run Task"):

- **Start Ayon Server** - Launches the AYON server via Docker
- **Stop Ayon Server** - Stops the AYON server
- **Start Kitsu Server** - Launches Kitsu production tracking
- **Stop Kitsu Server** - Stops Kitsu server
- **Run Ayon Core Tests** - Executes core library tests
- **Run Ayon Maya Tests: Unit** - Maya addon unit tests
- **Run Ayon Maya Tests: Integration** - Maya addon integration tests

### Running Tests

```bash
# Run all tests for a specific component
cd ynput-ayon-core
poetry run pytest

# Run tests with coverage
poetry run pytest --cov=ayon_core

# Run specific test files
poetry run pytest tests/test_specific_module.py
```

### Code Quality

The workspace is configured with automatic code formatting and linting:

- **Ruff** handles Python linting and formatting
- **Black** compatibility for consistent Python style
- **Prettier** for JavaScript/TypeScript formatting
- **ESLint** for JavaScript/TypeScript linting

Code is automatically formatted on save. To manually format:

```bash
# Python files
ruff format .
ruff check --fix .

# JavaScript/TypeScript files
npx prettier --write .
```

## 🐛 Debugging

### Python Debugging

1. Set breakpoints in VS Code by clicking in the left margin
2. Use the "Run and Debug" panel (`Ctrl+Shift+D`)
3. Select the appropriate debug configuration
4. Start debugging with `F5`

### Docker Debugging

Monitor container logs:

```bash
# AYON server logs
docker compose -f ../ayon-docker/docker-compose.yml logs -f

# Kitsu server logs
docker compose -f ../kitsu-docker/docker-compose.yml logs -f
```

## 📝 Configuration

### Workspace Settings

Key workspace settings are defined in `.vscode/ayon-dev.code-workspace`:

- **Python paths** - Configured for cross-project imports
- **Formatting** - Automatic formatting on save
- **File exclusions** - Optimized for performance
- **Theme & UI** - Consistent development environment

### Per-Project Settings

Each repository can override workspace settings with local `.vscode/settings.json` files.

### Environment Variables

Create `.env` files in relevant projects:

```bash
# Example .env for ayon-docker
AYON_VERSION=latest
AYON_PORT=5000
DATABASE_URL=postgresql://localhost/ayon
```

## 🔍 Troubleshooting

### Common Issues

**VS Code doesn't recognize Python paths:**

- Ensure virtual environments are activated
- Check `python.analysis.extraPaths` in workspace settings
- Restart the Python language server: `Ctrl+Shift+P` → "Python: Restart Language Server"

**Docker containers won't start:**

- Check if ports are already in use: `netstat -tulpn | grep :5000`
- Ensure Docker daemon is running: `sudo systemctl status docker`
- Clear Docker cache: `docker system prune`

**Import errors between projects:**

- Verify all projects are in the workspace `folders` array
- Check that virtual environments are properly set up
- Ensure `python.analysis.extraPaths` includes all necessary paths

**Performance issues:**

- Close unused editor tabs
- Exclude large directories in `files.exclude` settings
- Disable extensions you're not actively using

### Getting Help

1. **VS Code Command Palette** (`Ctrl+Shift+P`) - Access all commands
2. **Problems Panel** (`Ctrl+Shift+M`) - View linting errors
3. **Output Panel** - Check extension logs
4. **Terminal** (`Ctrl+``) - Run commands directly

## 📚 Additional Resources

- [AYON Documentation](https://ayon.ynput.io/)
- [VS Code Python Guide](https://code.visualstudio.com/docs/python/python-tutorial)
- [Docker Documentation](https://docs.docker.com/)
- [Poetry Documentation](https://python-poetry.org/docs/)
- [Ruff Configuration](https://docs.astral.sh/ruff/)

## 📄 License

This workspace configuration is part of the AYON project. See individual repositories for their respective licenses.
