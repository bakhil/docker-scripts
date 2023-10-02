# Docker scripts
These are a few scripts that make it easy for someone unfamiliar with docker containers
to easily run jobs on a server used by 10-15 people.
When a user runs the main script (`jupyter_container`), it creates a docker container
for them which can be used to submit jobs.
See the output of `jupyter_container -h` for the default image it builds on, and how to change it.
Later calls to `jupyter_container` make use of this same container, unless the user stops the container
(in which case it gets committed as an image and can be restarted where they left off)
or the user starts a fresh container.

Based on the needs of [where it was deployed](https://publish.illinois.edu/varshney/),
the script is named `jupyter_container` and the containers come with a jupyter notebook
running in them. But you can submit any job to the container (unrelated to jupyter) using the `--command` option.
Alternatively, you can change the "Dockerfile" on lines [163 to 177](https://github.com/bakhil/docker-scripts/blob/main/jupyter_container#L163)
of `jupyter_container`.

The other script `docker_overlay_stats` is for checking the disk usage statistics of the
containers running on the system.
The output of `docker system df` seems to diverge quite a bit compared to the actual
space it consumes (see [this issue](https://forums.mobyproject.org/t/overlay2-consuming-way-more-disk-space-than-reported-by-docker-system-df/564/9)
or [this one](https://github.com/moby/moby/issues/38848) or [this one](https://github.com/docker/cli/issues/942)).
So this script gives this data by using `du` on the `overlay` folder
docker uses.
I'm not sure if [`du` gives accurate answers](https://stackoverflow.com/a/52479005),
so please make sure you know what you're doing before taking irreversible actions based on
the output of this script. Run `docker_overlay_stats -h` for a list of options.
