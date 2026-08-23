# Vendor Boot Ramdisk Fix

A comprehensive solution for fixing and optimizing vendor boot ramdisk configurations on Android devices.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Configuration](#configuration)
- [Contributing](#contributing)
- [License](#license)
- [Support](#support)

## Overview

This project provides tools and utilities to diagnose, repair, and optimize vendor boot ramdisk issues commonly encountered in Android device development and customization. It addresses compatibility issues, boot failures, and ramdisk corruption problems.

## Features

- 🔧 Automated vendor boot ramdisk detection and repair
- 📋 Comprehensive diagnostics and logging
- 🚀 Quick boot optimization
- 🛡️ Backup and recovery functionality
- 📱 Support for multiple Android device architectures
- ⚙️ Configurable repair profiles
- 🔍 Detailed error reporting and troubleshooting

## Requirements

- **Operating System**: Linux/macOS/Windows with WSL
- **Tools**: 
  - Android SDK Platform Tools
  - Relevant device drivers
  - Build tools (if compiling from source)
- **Hardware**: USB cable for device connection (for direct testing)
- **Permissions**: Root/Administrator access for certain operations

## Installation

### Clone the Repository

```bash
git clone https://github.com/mkvenompro/vendor_boot_ramdisk_fix.git
cd vendor_boot_ramdisk_fix
```

### Setup

```bash
# Give execute permissions
chmod +x *.sh

# Install dependencies (if applicable)
./install.sh
```

## Usage

### Basic Usage

```bash
# Run the main fix utility
./vendor_boot_ramdisk_fix.sh

# Display help and available options
./vendor_boot_ramdisk_fix.sh --help
```

### Common Operations

**Diagnose Issues:**
```bash
./vendor_boot_ramdisk_fix.sh --diagnose
```

**Apply Fix:**
```bash
./vendor_boot_ramdisk_fix.sh --fix
```

**Backup Current Configuration:**
```bash
./vendor_boot_ramdisk_fix.sh --backup <backup-name>
```

**Restore from Backup:**
```bash
./vendor_boot_ramdisk_fix.sh --restore <backup-name>
```

## Configuration

Configuration files are typically located in the `config/` directory. Customize settings based on your device model and requirements:

- **Device Profile**: Select appropriate device model configuration
- **Repair Profile**: Choose from standard, aggressive, or custom repair modes
- **Logging Level**: Set verbosity for diagnostic output
- **Backup Location**: Specify where backups should be stored

Example configuration:
```
DEVICE_MODEL=model_name
REPAIR_MODE=standard
LOG_LEVEL=verbose
BACKUP_DIR=./backups
```

## Contributing

Contributions are welcome! To contribute:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-fix`)
3. **Commit** your changes (`git commit -m 'Add amazing fix'`)
4. **Push** to the branch (`git push origin feature/amazing-fix`)
5. **Open** a Pull Request with a clear description

Please ensure your contributions:
- Follow the existing code style
- Include appropriate documentation
- Add tests for new features
- Update the README if necessary

## License

This project is licensed under the MIT License. See the LICENSE file for details.

## Support

### Troubleshooting

If you encounter issues:

1. Check the [GitHub Issues](https://github.com/mkvenompro/vendor_boot_ramdisk_fix/issues) for similar problems
2. Review the diagnostic output and logs
3. Consult the troubleshooting section in the documentation
4. Create a new issue with detailed error information and logs

### Getting Help

- **Documentation**: Check the `/docs` directory for detailed guides
- **Issues**: Open a GitHub Issue with your problem
- **Discussions**: Use GitHub Discussions for general questions

---

**Note**: Always backup your current configuration before applying any fixes. This tool modifies critical system files.

**Disclaimer**: Use at your own risk. Improper use may result in device malfunction. Ensure you understand the implications before proceeding.
