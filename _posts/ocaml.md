---
title: "OCaml and OPAM"
image:
  path: /assets/img/logo_opam.png
...

## Profiling OCaml Performance

Linux `perf` can profile OCaml executables and collect call-graph information:

| Description                      | Command                                                                  |
| -------------------------------- | ------------------------------------------------------------------------ |
| Record a profile                 | `sudo perf record --call-graph lbr -- OCAML_EXECUTABLE`                  |
| Record a profile to a named file | `sudo perf record --call-graph lbr -o perf.data.old -- OCAML_EXECUTABLE` |
| Compare two profile files        | `perf diff perf.data.old perf.data`                                      |

A detailed guide is available in
[Profiling an OCaml project](https://ocaml-explore.netlify.app/workflows/profiling-your-project/).

## OPAM

### Main Commands

| Description                        | Command                                                        |
| ---------------------------------- | -------------------------------------------------------------- |
| Create a global switch             | `opam switch create SWITCH_NAME 4.14.1`                        |
| Create a local switch              | `opam switch create . 4.14.1`                                  |
| Remove a switch                    | `opam switch remove SWITCH_NAME`                               |
| Export a switch                    | `opam switch export FILE_EXPORT.txt --switch SWITCH_NAME`      |
| Import a switch                    | `opam switch import FILE_EXPORT.txt --switch SWITCH_NAME`      |
| Install only dependencies          | `opam install --deps-only PACKAGE_NAME`                        |
| Pin a Git branch                   | `opam pin add https://github.com/PATH_TO_REPO.git#BRANCH_NAME` |
| Install a specific package version | `opam install PACKAGE_NAME=VERSION`                            |

### Create an Empty Switch for Coq

1. Create a switch:

   ```bash
   opam switch create SWITCH_NAME 4.14.1
   ```

2. Select the switch and load its environment:

   ```bash
   opam switch SWITCH_NAME
   eval "$(opam env)"
   ```

3. Install dependencies:

   ```bash
   opam install --deps-only coq-core coq-elpi ocamlformat
   ```

4. Configure the project:

   ```bash
   ./configure
   ```

   - Enter `/home/USER/.opam/SWITCH_NAME/bin` as the executable directory.
   - For subsequent configuration paths, append the suggested suffix without
     replacing the `.bin` directory.

5. Build and install:

   ```bash
   make world
   dune install coq-core coq-stdlib coqide-server
   ```

### Resolve OCaml LSP Errors

If an OCaml project does not use Dune, Merlin may require a `.merlin` file:

```bash
make .merlin
touch dune-project
```

Add the fallback option in `.vscode/settings.json`:

```json
{
  "ocaml.server.args": [
    "--fallback-read-dot-merlin"
  ]
}
```

### Resolve a `Vnorm` Error in Coq-Elpi

```bash
git clean -dfx
```

Use this command carefully: it permanently removes untracked and ignored files.

### Exported OPAM Switch

A reference switch configuration is available
[here]({{site.baseurl}}/assets/lib/opam_switch_coq_elpi.txt).

### Useful Links

- [OPAM documentation](https://opam.ocaml.org/doc/FAQ.html)