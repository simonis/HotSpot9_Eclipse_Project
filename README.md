# My HotSpot project for Eclipse

Clone this repository right into your hostspot source tree. This can't be done directly, because the hotspot repository obviously already contains files. Here's how it works. First we clone this repository into an arbitrary location (called `${GIT_ROOT}` in this example):

```
cd ${GIT_ROOT}
git clone https://github.com/simonis/HotSpot_Eclipse_Project.git
```

Now change into root directory of your `hotspot` repository (called `${HOTSPOT_ROOT}` in this example) and copy the project there:

```
cd ${HOTSPOT_ROOT}
cp -r ${GIT_ROOT}/HotSpot_Eclipse_Project/.git .
git reset --hard HEAD
git pull
```
If you've already imported this project before, you may want to edit/change its name:

```
vi src/.project 
```

Now you can import the project into Eclipse:

```
File -> Import -> General -> Existing Projects into Workspace -> un-click "Copy project into workspace"
```

If you have the Git-plugin installed, Eclipse will detect this Ecplicse project (the one we've just copied) and put small question marks on all your HotSpot source files and directories because they aren't tracked by Git. To fix this, disable Git for your new `jdk9-hs-copm` HotSpot project:

```
Project (jdk9-hs) -> <right-click> -> Team -> Disconnect
```

Finally, we have to adjust the output directory in the project to match your local build output directry (this is necessary in order to index the generated hotspot source files):

```
Project (jdk9-hs) -> <right-click> -> Properties -> Resource -> Linked Resources -> (tab) Path Variables -> (select) OUTPUT_ROOT -> Edit
```

Set `OUTPUT_ROOT` to the OpenJDK build root directory (i.e. the one from which you've called configure or the one which contains the `images/jdk` build results). This directory should contain the generated hotspot sources under `hotspot/variant-server/gensrc`. If the name of this directory should change, you can adjust it in the `Linked Resources` tab.

Finally, you can select an active build configuration (i.e. `linux-amd64-dbg`) from:

```
Project (jdk9-hs) -> Build configurations -> Set active
```

For some unknown reason, the Eclipse CDT Indexer sometimes forgets your active build configuration. The following bug tracks this issue: https://bugs.eclipse.org/bugs/show_bug.cgi?id=205299. If you want to change your configuration frequently, the following setup is recommanded:

```
Window -> Preferences -> C/C++ -> Indexer -> Build configuration for the indexer -> (choose) "Use active build configuration"
Project (jdk9-hs) -> <right-click> -> Properties -> C/C++ General -> Code Analysis -> Indexer -> Build configuration for the indexer -> (check) "Use active build configration" and "Reindex project on change of active build configuration"
```
