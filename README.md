# JuLibAtoms

This repository contains a Julia `Pkg3` project specifying a 
Julia (1.0+) environment that interacts seemlessly with ASE and libatoms/QUIP.
The purpose is to be able to exactly reproduce a Julia environment on different
machines and in the libatoms docker.

The libatoms docker has two default Julia 1.x environments installed: `v1.1` and 
`JuLibAtoms`. The `JuLibAtoms` environment is a clone of this repository, 
while `v1.1` is a copy of `JuLibAtoms` created at the time of generating the 
docker container. For now, the recommended workflow is to not modify the `v1.1` 
environment, but to obtain an updated `JuLibAtoms` environment one should 
(from within the running docker)
```
cd ${JULIA_DEPOT_PATH}/environments/JuLibAtoms
git pull
julia -e 'using Pkg; Pkg.activate("."); Pkg.instantiate(); Pkg.resolve()'
```
And then to start Julia with 
```
julia -i -e 'using Pkg(); Pkg.activate("JuLibAtoms")'
```
Or, alternatively one can start Julia as usual, then switch to the 
Pkg REPL by pressing `]` and then type `activate JuLibAtoms`.

Once `JuLibAtoms` is activated, one can modify the environment e.g. by 
adding and removing packages, or pinning specific versions. These changes 
can then be pushed to the `JuLibAtoms` git repository.

Any changes to `JuLibAtoms` will become part of the `v1.1` environment 
(or subsequently `v1.x`) the next time the docker image is built.

Similarly, one can create different branches of `JuLibAtoms` and checkout 
and resolve the relevant branch in the running docker. The new package
manager no longer clones entire repositories and therefore it becomes 
fairly efficient in updating the Julia environment.