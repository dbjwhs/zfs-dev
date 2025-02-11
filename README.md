# OpenZFS Development Environment Setup

This document describes how to set up a development environment for OpenZFS using Docker on macOS with Apple Silicon processors.

## Current Setup

### Prerequisites
- macOS with Apple Silicon (M1/M2/M3/M4)
	- *tested on M3 Max Macbook*
- Docker Desktop for Mac installed and running

### Setting Up the Development Environment

If you want to create the docker image outside of this repository then create a new directory
for the ZFS development environment and save the [Dockerfile](docker/Dockerfile) to your new directory,
in any event continue to next step.

### Building and Running the Container

From the [docker](docker/) directory;

1. Build the Docker image:
```bash
docker build -t zfs-dev .
```

2. Run the container:
```bash
docker run -it -v $(pwd):/workspace zfs-dev /bin/bash
```

The `-v $(pwd):/workspace` flag creates a volume mount between your host machine's current directory and the container's
`/workspace` directory, allowing file sharing between host and container.

### Current Testing Capabilities

Currently, we can run the userland test suite using `ztest`. To run it:

```bash
cd /zfs
./ztest
```

#### What ztest Does
- Creates artificial workloads on ZFS
- Tests userland components without requiring kernel modules
- Creates in-memory test pools
- Exercises various ZFS operations
- Particularly effective at finding:
  - Race conditions
  - Edge cases
  - Memory leaks
  - Data corruption issues
- Uses randomized workloads, making each test run unique

## Future Work

### Full ZFS Test Suite Setup

To run the complete ZFS test suite, we'll need to:

1. Modify the Docker configuration to run with extended privileges:
   - Need to run Docker with `--privileged` flag
   - Possibly need additional volume mounts for device access

2. Set up kernel module support:
   - Load ZFS kernel module in container
   - Configure module parameters
   - Set up device mapper support

3. Configure test environment:
   - Create physical test pools
   - Set up proper permissions
   - Configure test suite parameters

4. Run full test suite:
   ```bash
   cd /zfs/tests
   ./runfiles/linux.run
   ```

### Alternative Testing Approaches to Consider

1. VM-based testing:
   - Using VirtualBox or VMware
   - Full system access
   - More realistic testing environment
   - Easier kernel module handling

2. Host system testing:
   - Direct installation on Linux partition
   - Native performance
   - Full system access
   - More complex setup

## Contributing

The current setup allows for:
- Code reading and navigation
- Basic development work
- Userland testing via ztest
- Building and modifying OpenZFS code

Remember to:
- Work in the `/workspace` directory for persistent changes
- Commit changes to your fork of OpenZFS
- Follow the OpenZFS contribution guidelines

## Useful Resources

- [OpenZFS GitHub Repository](https://github.com/openzfs/zfs)
- [OpenZFS Documentation](https://openzfs.github.io/openzfs-docs/)
- [ZFS on Linux Wiki](https://zfsonlinux.org/)


## License

This project (not OpenZFS, this repository) is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
