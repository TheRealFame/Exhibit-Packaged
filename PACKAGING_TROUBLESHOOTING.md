# Packaging Troubleshooting Guide

This guide provides solutions for common issues when installing Exhibit via Debian package (.deb) or AppImage.

## Debian Package (.deb) Issues

### Installation Fails with Dependency Errors

If you encounter dependency errors during installation:

```sh
sudo apt-get install -f
```

This will attempt to fix broken dependencies.

### ModuleNotFoundError: No module named 'f3d'

The deb package requires Python dependencies to be installed separately:

```sh
pip install --break-system-packages f3d Wand
```

If you prefer not to use `--break-system-packages`, create a virtual environment:

```sh
python3 -m venv ~/.local/venvs/exhibit
source ~/.local/venvs/exhibit/bin/activate
pip install f3d Wand
```

Then modifyPATH to include the virtual environment when running exhibit.

### ImportError: undefined symbol: _PyThreadState_UncheckedGet

This error occurs when the Python version used to build f3d doesn't match your system Python version. Reinstall f3d to build it for your Python version:

```sh
pip install --break-system-packages --force-reinstall f3d
```

### XDG_DATA_HOME KeyError

If you encounter `KeyError: 'XDG_DATA_HOME'`, ensure the environment variable is set:

```sh
export XDG_DATA_HOME="$HOME/.local/share"
export XDG_CONFIG_HOME="$HOME/.config"
export XDG_CACHE_HOME="$HOME/.cache"
```

Or add these to your `~/.bashrc` or `~/.zshrc` file.

## AppImage Issues

### Permission Denied

Make the AppImage executable:

```sh
chmod +x Exhibit-x86_64.AppImage
```

### Fails to Launch with Python Module Errors

The AppImage relies on your system's Python and requires the same dependencies as the deb package:

```sh
pip install --break-system-packages f3d Wand
```

### Missing GTK4 or Adwaita Libraries

The AppImage requires GTK4 and libadwaita to be installed on your system:

```sh
sudo apt-get install libgtk-4-1 libadwaita-1-0
```

On other distributions, use your package manager to install GTK4 and libadwaita.

### Vulkan/OpenGL Errors

If you encounter Vulkan or OpenGL initialization errors, ensure your graphics drivers are properly installed and up to date.

## General Python Issues

### Externally Managed Environment Error

If you get an error about externally managed Python environment, use:

```sh
pip install --break-system-packages f3d Wand
```

Or use a virtual environment as described above.

### pip Not Found

Install pip:

```sh
sudo apt-get install python3-pip
```

## System-Specific Issues

### Ubuntu/Debian

Ensure you have the required system packages:

```sh
sudo apt-get update
sudo apt-get install python3 python3-pip libgtk-4-1 libadwaita-1-0
```

### Fedora/RHEL

```sh
sudo dnf install python3 python3-pip gtk4 libadwaita
```

### Arch Linux

```sh
sudo pacman -S python python-pip gtk4 libadwaita
```

## Getting Help

If you continue to experience issues:

1. Check the [GitHub Issues](https://github.com/Nokse22/Exhibit/issues) for similar problems
2. Create a new issue with:
   - Your Linux distribution and version
   - Python version (`python3 --version`)
   - Exact error messages
   - Steps to reproduce the issue
