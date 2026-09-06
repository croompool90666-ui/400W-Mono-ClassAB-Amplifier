# Usage and development notes

This document explains how to use the repository scripts and how the GitHub Actions KiCad workflow is intended to run.

Prerequisites
- KiCad 7.x (workflow uses image `kicad/kicad:7.0.10`).
- Docker (for running the same image locally) or a KiCad installation with `kicad-cli` available.

Running the helper script
- The repo includes a small helper script at `scripts/generate_gerbers.sh` intended as a placeholder for locally exporting KiCad outputs.
- The script is intentionally minimal and safe (it only echoes filenames and messages). To run it locally:

  chmod +x scripts/generate_gerbers.sh
  ./scripts/generate_gerbers.sh

- To make the script actually export BOMs/Gerbers, replace the echo lines with appropriate `kicad-cli` commands or run the commands shown in the CI workflow from within a KiCad-capable environment.

Running the same exports with Docker (approximation of the CI step)
- From the repo root (where the .kicad_pcb/.kicad_sch files live):

  docker run --rm -v "$PWD":/workspace -w /workspace kicad/kicad:7.0.10 sh -c '
    set -e
    schem=$(git ls-files "*.kicad_sch" | head -n1 || true)
    pcb=$(git ls-files "*.kicad_pcb" | head -n1 || true)
    if [ -n "$schem" ]; then
      kicad-cli sch export pdf "$schem" -o build_outputs/schematic.pdf
      kicad-cli sch export bom "$schem" -o build_outputs/bom.csv
    fi
    if [ -n "$pcb" ]; then
      mkdir -p build_outputs/gerbers
      kicad-cli pcb export gerbers "$pcb" -o build_outputs/gerbers/
      cd build_outputs && zip -r gerbers.zip gerbers || true
    fi
  '

Notes about CI
- The repository uses a GitHub Actions workflow (.github/workflows/kicad-build.yml) that runs the KiCad Docker export and then makes a release with the generated artifacts.
- Recent CI runs reported `Failed to load schematic file`. Possible causes:
  - The `kicad-cli` version may be picky about file contents or project layout. Verify the schematic file is a supported KiCad v7 file and not corrupted.
  - `kicad-cli` may expect to be run from within a KiCad project directory or may need additional files present (symbol libraries, etc.). Running the Docker command locally (above) helps reproduce.

Recommended next steps
1. Merge the tolerant workflow PR (branch `ci/kicad-tolerant-export`) which adds debug output and avoids failing the whole job on export errors; merging will also help gather better logs.
2. Run the Docker command shown above locally to reproduce the `Failed to load schematic file` error and gather the `kicad-cli` stderr output. That output will guide the next fix.
3. Optionally run `shellcheck` on scripts and add a lightweight linter job for shell scripts in CI.
4. If you want, I can open issues for the CI failure and for any TODOs I discover during further scanning.

If you'd like, I can:
- Run a deeper automated scan for TODO/FIXME and obvious typos and apply small fixes. (I have write access and can push non-workflow changes.)
- Create issues for the CI failure and the workflow PR so they are tracked.

