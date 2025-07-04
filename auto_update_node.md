# 🛠️ Script: update-nexus.sh – Automatically Update Nexus CLI
## Purpose: This script automates the process of updating the Nexus CLI (nexus-network binary) to a specific version from the official GitHub repository. It performs the following steps:

- Checks if the nexus-cli repository exists locally.
- If not, it automatically clones it from GitHub.
- Checks out the version you specify (e.g., v0.9.1).
- Builds the binary using cargo build --release.
- Copies the built binary to ~/.nexus/bin.
- Displays the actual version of the built binary for verification.
---
## 📄 How to create this script on your machine:
```bash
# Step 1: Create a new script file
nano ~/update-nexus.sh
```
## 📌 Paste the entire script content below into the file:
(replace this line with the actual content of the script you've already written)
```bash
#!/usr/bin/env bash

# update-nexus.sh: Automatically update Nexus CLI to a specified version
# Usage: ./update-nexus.sh <version>
# Example: ./update-nexus.sh v0.9.1

set -euo pipefail

if [[ "$#" -ne 1 ]]; then
  echo "Usage: $0 <version>"
  echo "Example: $0 v0.9.1"
  exit 1
fi

VERSION="$1"
REPO_URL="https://github.com/nexus-xyz/nexus-cli.git"
REPO_DIR="$HOME/nexus-cli"
BUILD_DIR="${REPO_DIR}/clients/cli"
BIN_DEST="$HOME/.nexus/bin"

# Clone the repo if it doesn't exist
if [[ ! -d "$REPO_DIR" ]]; then
  echo "📦 Repository not found at $REPO_DIR. Cloning from GitHub..."
  git clone "$REPO_URL" "$REPO_DIR"
else
  echo "📁 Repository already exists at $REPO_DIR"
fi

cd "$REPO_DIR"

echo "🔄 Fetching tags from remote..."
git fetch --tags

echo "🔀 Checking out version $VERSION..."
git checkout "$VERSION"

# Build the release binary
echo "⚙️  Building release in $BUILD_DIR..."
cd "$BUILD_DIR"
cargo build --release

# Verify the binary exists
BIN_SRC="${BUILD_DIR}/target/release/nexus-network"
if [[ ! -f "$BIN_SRC" ]]; then
  echo "❌ Binary not found at $BIN_SRC"
  exit 1
fi

# Create destination directory if it doesn't exist
mkdir -p "$BIN_DEST"

echo "📁 Copying binary to $BIN_DEST..."
cp "$BIN_SRC" "$BIN_DEST/"

# Check the built version
echo "🔎 Verifying built version:"
"$BIN_SRC" -V

# Done
echo "✅ Nexus CLI has been successfully updated to $VERSION!"

```
## Next step
```bash
# Step 2: Make the script executable
chmod +x ~/update-nexus.sh
```
```bash
# Step 3: Run the script with the desired version
~/update-nexus.sh v0.9.1
```
---
# 🛠️ Dependency Setup for Nexus CLI
## If you haven’t installed the required dependencies yet, follow the steps below.
--- 
## ✅ 1. For Linux (Ubuntu/Debian)
- 🔧 Install Rust (includes both cargo and rustc)
```bash
# Update package list and install required system packages
sudo apt update
sudo apt install -y curl build-essential pkg-config libssl-dev

# Install Rust using the official installer
curl https://sh.rustup.rs -sSf | sh
```
- 👉 When prompted, choose:
```
1) Proceed with installation (default)
```
- ✅ Reload your shell after installation:
```bash
source $HOME/.cargo/env
```
- ✅ Verify installation:
```bash
rustc --version
cargo --version
```
- You should see output like:
```scss
rustc 1.85.0 (xxxxxxx)
cargo 1.85.0 (xxxxxxx)
```
