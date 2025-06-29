# dotfiles

## clang-tidy

When included in the CMakeLists.txt

```cmake
set(CMAKE_CXX_CLANG_TIDY "clang-tidy;--format-style=file")
```

Typical build procedure

```shell
mkdir build
cd build

# Generate compile_commands.json
cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=ON ..

cd ..
```

Manual execution

```shell
# Basic Checks
clang-tidy -p build --format-style=file ${TARGET_FILES}

# Include changes
clang-tidy -p build --format-style=file ${TARGET_FILES} --fix

# Ignoring specific warnings
clang-tidy -p build --format-style=file ${TARGET_FILES} \
  --checks='-readability-identifier-length'

# Check only specific warnings
clang-tidy -p build --format-style=file ${TARGET_FILES} \
  --checks='-*,misc-include-cleaner'

# Targeting multiple files (example of 4 parallel)
find ${TARGET_DIR}/ -name '*.h' -o -name '*.cpp' | \
  xargs -I{} --max-procs=4 \
    clang-tidy -p build --format-style=file {}
```

If necessary, change the .clang-tidy for each project.

```diff
   misc-include-cleaner.DeduplicateFindings: 'true'
-  misc-include-cleaner.IgnoreHeaders: ''
+  misc-include-cleaner.IgnoreHeaders: 'Eigen/.*;opencv2/.*;yaml-cpp/.*;nlohmann/.*;g2o/types/.*;fbow/.*;'
   misc-non-private-member-variables-in-classes.IgnoreClassesWithAllMemberVariablesBeingPublic: 'false'
```
