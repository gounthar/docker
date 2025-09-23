# Step-by-Step Plan: Adding riscv64 Architecture Support

Each task is atomic, testable, and focused on a single concern. Complete and test each before moving to the next.

---

## 1. Verify riscv64 Base Image Availability ✅
**Start:** Identify all base images used in Dockerfiles (Alpine, Debian, RHEL, etc.).  
**End:** Confirm that official riscv64 images exist for each base image and document any that are missing.

**Result:**  
- riscv64 is supported for Alpine only.  
- riscv64 is NOT supported for Debian bookworm or RHEL UBI9.  
- All further riscv64 tasks apply to Alpine only unless otherwise noted.

---

## 2. Update `docker-bake.hcl` Build Matrix
**Start:** Open `docker-bake.hcl`.  
**End:** Add `linux/riscv64` to the `platforms` list for each relevant build target (**Alpine only**; Debian bookworm and RHEL do not support riscv64).

---

## 3. Update Makefile Build Targets
**Start:** Open `Makefile`.  
**End:** Add `riscv64` to any architecture lists or build targets that enumerate supported platforms.

---

## 4. Update CI/CD Pipeline for riscv64
**Start:** Open `Jenkinsfile` and any other CI config (e.g., `build-windows.yaml`).  
**End:** Add `linux/riscv64` to the build/test matrix so CI attempts to build and test riscv64 images.

---

## 5. Enable QEMU Emulation for riscv64 in CI (if needed)
**Start:** Check if native riscv64 runners are available in CI.  
**End:** If not, configure QEMU emulation in CI to enable riscv64 builds and document the setup.

---

## 6. Test Dockerfile Compatibility for riscv64
**Start:** Attempt to build each Dockerfile for riscv64 using Docker Buildx (locally or in CI).  
**End:** Document any build failures or required changes for riscv64 compatibility.

---

## 7. Update/Adapt Dockerfiles for riscv64 (if needed)
**Start:** For any Dockerfile that fails to build for riscv64, make minimal, architecture-specific changes (e.g., alternate binaries, dependencies, or workarounds).  
**End:** All Dockerfiles build successfully for riscv64.

---

## 8. Ensure Test Scripts are Architecture-Agnostic
**Start:** Review test scripts in `tests/` for hardcoded architectures or platform-specific logic.  
**End:** Refactor scripts to support riscv64, or add riscv64-specific test cases if necessary.

---

## 9. Run All Tests on riscv64 Images
**Start:** Trigger the full test suite (unit and E2E) on riscv64 images in CI.  
**End:** All tests pass, or failures are documented and triaged.

---

## 10. Update Policy/Metadata Files
**Start:** Open `policy/manifest/config.json` and `policy/metadata/config.json`.  
**End:** Add riscv64 to the list of supported architectures if tracked in these files.

---

## 11. Update Documentation for riscv64 Support
**Start:** Open `README.md`, `HACKING.adoc`, and any other relevant docs.  
**End:** Add instructions, notes, and caveats for riscv64 support, including any limitations or special requirements.

---

## 12. Update CHANGELOG.md
**Start:** Open `CHANGELOG.md`.  
**End:** Add an entry describing the addition of riscv64 support.

---

## 13. Submit Pull Request and Request Review
**Start:** Open a new pull request with all changes.  
**End:** Ensure all CI checks pass and request code review per contribution guidelines.

---

**Note:** Each step should be completed and validated before proceeding to the next.
