# Repository Architecture Analysis

This repository is structured to build and test Jenkins Docker images for multiple operating systems and configurations. Below is a detailed breakdown of the file and folder structure, the purpose of each part, and how you could add support for a new architecture such as RISC-V 64.

---

## 1. File & Folder Structure

```
.
├── .git-blame-ignore-revs
├── .gitignore
├── .gitmodules
├── .hadolint.yml
├── build-windows.yaml
├── CHANGELOG.md
├── config.json
├── CONTRIBUTING.md
├── docker-bake.hcl
├── graph.mmd
├── graph.mmd.png
├── graph.mmd.svg
├── HACKING.adoc
├── jdk-download-url.sh
├── jdk-download.sh
├── jenkins-plugin-cli.ps1
├── jenkins-plugin-cli.sh
├── jenkins-support
│   ├── ... (support scripts/modules)
├── jenkins-support.psm1
├── jenkins.ps1
├── jenkins.sh
├── Jenkinsfile
├── LICENSE.txt
├── make.ps1
├── Makefile
├── README.md
├── SECURITY.md
├── tini_pub.gpg
├── alpine/
│   └── hotspot/
│       └── Dockerfile
├── compose/
│   └── config.json
├── debian/
│   ├── bookworm/
│   │   └── hotspot/
│   │       └── Dockerfile
│   └── bookworm-slim/
│       └── hotspot/
│           └── Dockerfile
├── policy/
│   ├── manifest/
│   │   └── config.json
│   └── metadata/
│       └── config.json
├── rhel/
│   └── ubi9/
│       └── hotspot/
│           └── Dockerfile
├── target/
├── tests/
│   ├── functions.bats
│   ├── functions.Tests.ps1
│   ├── plugins-cli.bats
│   ├── plugins-cli.Tests.ps1
│   ├── runtime.bats
│   ├── runtime.Tests.ps1
│   ├── test_helpers.bash
│   ├── test_helpers.psm1
│   ├── functions/
│   │   ├── Dockerfile
│   │   └── Dockerfile-windows
│   ├── plugins-cli/
│   │   ├── Dockerfile
│   │   ├── Dockerfile-windows
│   │   ├── custom-war/
│   │   │   ├── Dockerfile
│   │   │   ├── Dockerfile-windows
│   │   │   └── WEB-INF/
│   │   │       └── plugins/
│   │   │           └── my-happy-plugin.hpi
│   │   ├── java-opts/
│   │   │   └── Dockerfile
│   │   ├── no-war/
│   │   │   ├── Dockerfile
│   │   │   ├── Dockerfile-windows
│   │   │   └── plugins.txt
│   │   ├── pluginsfile/
│   │   │   ├── Dockerfile
│   │   │   ├── Dockerfile-windows
│   │   │   └── plugins.txt
│   │   ├── ref/
│   │   │   ├── Dockerfile
│   │   │   └── Dockerfile-windows
│   │   └── update/
│   │       ├── Dockerfile
│   │       └── Dockerfile-windows
│   ├── test_helper/
│   │   ├── bats-assert/
│   │   └── bats-support/
│   └── upgrade-plugins/
│       ├── Dockerfile
│       └── Dockerfile-windows
├── tools/
│   ├── hadolint
│   └── shellcheck
├── updatecli/
│   ├── config.json
│   ├── values.github-action.yaml
│   └── updatecli.d/
│       ├── alpine.yaml
│       ├── debian.yaml
│       ├── git-lfs.yaml
│       ├── hadolint.yaml
│       ├── jdk-lts.yaml
│       ├── jdk17.yaml
│       ├── jdk21.yaml
│       ├── jdk25.yaml
│       └── shellcheck.yaml
├── windows/
│   └── windowsservercore/
│       └── hotspot/
│           └── Dockerfile
```

---

## 2. What Each Part Does

### Top-Level Files

- **Jenkinsfile, Makefile, build-windows.yaml**: Define CI/CD pipeline steps, build logic, and automation for building and testing images.
- **docker-bake.hcl**: Configuration for Docker Buildx bake, enabling multi-platform builds.
- **README.md, CONTRIBUTING.md, SECURITY.md, HACKING.adoc, CHANGELOG.md**: Documentation and guidelines.
- **config.json**: Likely global configuration for builds or image metadata.
- **.hadolint.yml**: Linting rules for Dockerfiles.
- **jdk-download*.sh, jenkins*.sh/ps1**: Scripts for downloading JDKs and Jenkins, used in Dockerfiles or build steps.
- **tini_pub.gpg**: GPG key for verifying tini, a minimal init system used in containers.

### OS/Distribution Folders

- **alpine/**, **debian/**, **rhel/**, **windows/**: Each contains subfolders for specific OS versions and variants (e.g., `bookworm`, `ubi9`, `windowsservercore`), each with a `hotspot/` subfolder containing a `Dockerfile` for building Jenkins images on that base.
- **compose/**: Contains configuration for Docker Compose setups.
- **policy/**: Contains manifest and metadata configurations, likely for image policy or compliance.

### Test Infrastructure

- **tests/**: Contains BATS and PowerShell test scripts, as well as Dockerfiles for test images and test helpers.
- **tests/plugins-cli/**, **tests/functions/**, etc.:** Subfolders for specific test scenarios or plugins.

### Tooling

- **tools/**: Contains static analysis tools (e.g., hadolint, shellcheck) used in CI.
- **updatecli/**: Contains configuration for updatecli, a tool for automating dependency updates and version bumps.

### Miscellaneous

- **target/**: Likely a build output directory (may be gitignored).
- **graph.mmd, graph.mmd.png, graph.mmd.svg**: Mermaid diagram and its rendered images, likely visualizing the build or image dependency graph.

---

## 3. How Each Part Contributes

- **Dockerfiles in OS folders**: Define how Jenkins is built for each OS/variant/architecture.
- **Build scripts and Makefile**: Orchestrate the build process, including multi-arch builds.
- **docker-bake.hcl**: Central place to define all build targets, platforms, and build matrix for Docker Buildx.
- **CI/CD files (Jenkinsfile, build-windows.yaml)**: Automate the build, test, and publish process for all supported images.
- **Test scripts**: Ensure images work as expected across all supported platforms and configurations.
- **updatecli/**: Keeps dependencies and base images up to date automatically.

---

## 4. Envisioning Addition of a New Architecture (RISC-V 64)

### a. Dockerfile Support

- **Check if base images (e.g., Alpine, Debian, RHEL) support RISC-V 64.**
- **If supported, Dockerfiles may not need changes** unless there are architecture-specific tweaks (e.g., different binaries, dependencies, or workarounds).

### b. Build Matrix Update

- **docker-bake.hcl**: Add `riscv64` to the `platforms` list for relevant targets.
- **Makefile / build scripts**: Ensure `riscv64` is included in build targets.
- **CI/CD (Jenkinsfile, build-windows.yaml)**: Update to trigger builds for `linux/riscv64` (and possibly test jobs).

### c. Test Infrastructure

- **tests/**: Ensure tests are architecture-agnostic or add RISC-V 64 specific test cases if needed.
- **CI/CD**: Add or update test jobs to run on RISC-V 64 images (may require QEMU emulation or native runners).

### d. Documentation

- **README.md, HACKING.adoc, etc.**: Document RISC-V 64 support, build instructions, and any known issues or limitations.

### e. Policy/Metadata

- **policy/manifest/config.json, policy/metadata/config.json**: Update to reflect new architecture support if these files track supported platforms.

---

## 5. Example: Adding RISC-V 64 Support

### 1. Update `docker-bake.hcl`

```hcl
target "debian-bookworm-hotspot" {
  platforms = [
    "linux/amd64",
    "linux/arm64",
    "linux/riscv64"   # <-- Add this line
  ]
  ...
}
```

### 2. Update CI/CD

- In `Jenkinsfile` or GitHub Actions, add `linux/riscv64` to the build matrix.
- Ensure QEMU is set up for emulation if native runners are not available.

### 3. Test

- Run all existing tests on the new architecture.
- Add architecture-specific tests if needed.

### 4. Document

- Update documentation to mention RISC-V 64 support and any caveats.

---

## 6. Mermaid Diagram: High-Level Build & Test Flow

```mermaid
flowchart TD
    A[Source Code & Dockerfiles] --> B[Build Matrix (docker-bake.hcl)]
    B --> C[CI/CD Pipeline (Jenkinsfile, Makefile)]
    C --> D[Docker Buildx: Multi-Arch Images]
    D --> E[Tests (BATS, PowerShell)]
    E --> F[Publish Images]
    F --> G[Documentation & Metadata]
```

---

## 7. Summary Table

| Component                | Purpose                                              | RISC-V 64 Addition Step                |
|--------------------------|-----------------------------------------------------|----------------------------------------|
| Dockerfiles (per OS)     | Build Jenkins for each OS/variant                   | Ensure base image supports riscv64     |
| docker-bake.hcl          | Build matrix for all images/architectures           | Add `linux/riscv64` to platforms       |
| Makefile/build scripts   | Orchestrate builds                                  | Add riscv64 to targets                 |
| Jenkinsfile/CI           | Automate build/test/publish                         | Add riscv64 to build/test matrix       |
| tests/                   | Validate images                                     | Run tests on riscv64                   |
| updatecli/               | Dependency updates                                  | No change needed                       |
| Documentation            | Usage, build, and support info                      | Add riscv64 instructions/notes         |
| policy/metadata          | Track supported platforms                           | Add riscv64 if tracked                 |

---

## 8. Conclusion

The repository is well-structured for multi-architecture Docker image builds. Adding RISC-V 64 support involves:

- Ensuring base images are available for RISC-V 64.
- Updating the build matrix and CI/CD to include the new architecture.
- Running and possibly adapting tests.
- Documenting the new support.

This approach will extend compatibility and future-proof the project for emerging platforms.
