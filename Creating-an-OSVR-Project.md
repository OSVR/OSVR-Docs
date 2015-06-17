This document is specifically about creating a project/repo within the umbrella of OSVR, not a project that uses OSVR (though you might want to follow similar guidelines).

We try to follow the guidance of [Karl Fogel's Producing Open Source Software book][producingoss] in general, using the in-revision updated edition online, since it captures a good deal of valuable knowledge from a range of established projects. Questions can be directed to the [development mailing list][] for clarification or additional guidance.

[producingoss]:http://producingoss.com/
[development mailing list]:http://lists.getosvr.org/listinfo.cgi/osvr-devel-getosvr.org

## Separate project vs. existing project
We want people to be able to use the projects we publish. In general, if your addition is somehow separable, optional, or (especially) if it adds dependencies, the community tendency is to prefer a separate repository. This allows consuming projects to import their dependencies as Git submodules or similar "vendoring" setups, without bringing in extra baggage.

## Contents of a project
Note that this section assumes that the Apache Software License, version 2.0, is being used (which is the default).

For these files, the [OSVR-Core project](https://github.com/OSVR/OSVR-Core) is generally a good source of examples, though note that it might have somewhat longer/more comprehensive files due to its, well, *core*, role in the ecosystem.

### Human Docs
#### `README.md` - required
This shows up on the repo homepage on GitHub, and is often converted to HTML and included in binaries by build processes. There are several important parts to such a file - until we get a chance to document them, see [the starting a project section in Producing Open Source Software][producingoss-starting] and an existing one as an example.

[producingoss-starting]:http://producingoss.com/en/getting-started.html#starting-from-what-you-have

Note that unless it makes this file quite long, you can keep documentation directly in this file as well.

Subdirectories can also have `README.md` files, which show up on GitHub when that directory is viewed, but this is less important than the main one.

#### `CONTRIBUTING.md` - highly recommended
Having such a file is useful because it provides instructions on how to get involved and basic expectations. Additionally, using that particular name enables [GitHub integration with issues and pull requests](https://github.com/blog/1184-contributing-guidelines). Use an existing one as a template, though be sure to review it thoroughly - worse than a missing contributing document is a misleading one.

### License-related
#### `LICENSE` - required
This should be a verbatim copy of the full ASL2.0 - grab from OSVR-Core.

#### `NOTICE` - required (?)
Start with an example, updating it for the specific project. Also, in output, such a NOTICE file should contain all incorporated projects' NOTICE files and other required notices. Trying to find a nice tool to do this automatically, possibly from Android (since "open source components" screens there look consistent like they were auto-generated).

### Tool-related
#### `.gitignore`
If you're on Windows, you might have to create this as `.gitignore.` to get Explorer to let you name a file that. It should ignore all system-specific and generated files: you should be able to do a full "build" (whatever that means in the context of your project) and have `git status` show that it's clean. Helpful resources are existing projects that use the same technology, as well as these site:

- <https://github.com/github/gitignore>
- <https://www.gitignore.io/>

#### `.clang-format`
For C++-based projects, make sure to include a clang-format configuration file. You can use the one from OSVR-Core for consistency. Then, **make sure you run clang-format before each commit!** It keeps the diffs clean and readable.

## Source code files
### Boilerplate generator
For C headers and C++ source and header files, we have a boilerplate generator that takes care of producing include guards, stock license headers and file description headers, etc. <http://opengoggles.org/internal/generate-cpp-boilerplate/> It's somewhat crude at the moment and probably ripe for replacement, but it does work pretty well, especially if you integrate it with your send-to menu on Windows or script it on posix.

### Each source file: stock ASL2.0 copyright and license header
The general format of this is as follows, note that you'll want to prefix each line (or wrap the block) in language-specific comment characters: a text editor capable of search/replace with regex is handy here (`sed 's:^:// :'` for the command-line users with C++-style comments...). Also, **do not include the brackets in the final text, they should be replaced along with their placeholder contents**

```
Copyright [yyyy] [name of copyright owner/initial author] and OSVR community

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```