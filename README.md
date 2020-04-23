# pymake 

deps resolver/build system to be.
```
    def with_cycle():
        deps_resolver = DepsResolver()
        deps_resolver.add_task("publish", ["build-release"], "print publish")
        deps_resolver.add_task("build-release", ["nim-installed"], "print exec command to build release mode")
        deps_resolver.add_task("nim-installed", ["curl-installed"], "print curl LINK | bash")
        deps_resolver.add_task("curl-installed", ["publish", "apt-installed"], "apt-get install curl")
        deps_resolver.add_task("apt-installed", [], "code to install apt...")
        deps_resolver.run_task("publish")
```
when invoke `without_cycle`
```
executing  code to install apt...
executing  apt-get install curl
executing  print curl LINK | bash
executing  print exec command to build release mode
executing  print publish
```

```
    def without_cycle():
        deps_resolver = DepsResolver()
        deps_resolver.add_task("publish", ["build-release"], "print publish")
        deps_resolver.add_task("build-release", ["nim-installed"], "print exec command to build release mode")
        deps_resolver.add_task("nim-installed", ["curl-installed"], "print curl LINK | bash")
        deps_resolver.add_task("curl-installed", ["apt-installed"], "apt-get install curl")
        deps_resolver.add_task("apt-installed", [], "code to install apt...")
        deps_resolver.run_task("publish")
```


when invoke `with_cycle`: 

```
cycle found: {'publish': 'curl-installed', 'build-release': 'publish', 'nim-installed': 'build-release', 'curl-installed': 'nim-installed', '__CYCLESTART__': 'publish'}
```