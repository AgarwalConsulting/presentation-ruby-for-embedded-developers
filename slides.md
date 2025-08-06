layout: true

.signature[@algogrit]

---

class: center, middle

# Ruby for Embedded Developers

Gaurav Agarwal

---

# Agenda

- Ruby is *versatile*

---

class: center, middle

![Me](assets/images/me.png)

Software Engineer & Product Developer

Director of Engineering & Founder @ https://codermana.com

ex-Tarka Labs, ex-BrowserStack, ex-ThoughtWorks

---

## As an instructor

- I promise to

  - make this class as interactive as possible

  - use as many resources as available to keep you engaged

  - ensure everyone's questions are addressed

---

## What I need from you

- Be vocal

  - Let me know if there are any audio/video issues ASAP

  - Feel free to interrupt me and ask me questions

- Be punctual

- Give feedback

- Work on the exercises

- Be *on mute* unless you are speaking

---
class: center, middle

## Class progression

![Learning Curve](assets/images/learning-curve.jpg)

---
class: center, middle

Here you are trying to *learn* something, while here your *brain* is doing you a favor by making sure the learning doesn't stick!

---

### Some tips

- Slow down => stop & think
  - listen for the questions and answer

- Do the exercises
  - not add-ons; not optional

- There are no dumb questions!

- Drink water. Lots of it!

---

### Some tips (continued)

- Take notes
  - Try: *Repetitive Spaced Out Learning*

- Talk about it out loud

- Listen to your brain

- *Experiment!*

---
class: center, middle

### 📚 Content ` > ` 🕒 Time

---
class: center, middle

## Show of hands

*Yay's - in Chat*

---
class: center, middle

## Why Ruby?

.content-credits[https://ruby.slides.agarwalconsulting.com/#19]

---
class: center, middle

Using **Ruby for embedded development** is uncommon, but can be a valid choice in some niche scenarios.

---
class: center, middle

## Reasons to Consider Ruby for Embedded Development

---
class: center, middle

### High Productivity

---

- **expressive, concise syntax** and powerful metaprogramming make development fast and readable.

- Useful for **prototyping**, **configuration scripts**, or **control logic** where development speed outweighs performance concerns.

---
class: center, middle

### **mruby: Lightweight Ruby**

---

- Compiles Ruby to C bytecode.

- Can be embedded into C/C++ programs.

- Small memory footprint (can run on devices with \~100-300KB RAM).

- Good for scripting within constrained environments.

---
class: center, middle

Implements a subset of Ruby (no full stdlib, no threads, no GC tuning)

---

| Feature               | CRuby (MRI)                   | mruby                                                    |
| --------------------- | ----------------------------- | -------------------------------------------------------- |
| **Gem Manager**       | `RubyGems` + `Bundler`        | `mgem` (for `mrbgems`)                                   |
| **Install Gems**      | Runtime (`gem install`)       | Build time (linked at compile)                           |
| **Popular Gems**      | Rails, Nokogiri, Puma, RSpec  | Custom gems like `mruby-io`, `mruby-print`, `mruby-time` |
| **Native Extensions** | Supported (`C`, `Rust`, etc.) | ❌ Not supported in the same way                          |
| **Dynamic Loading**   | ✅ (`require`, `load`)         | ⚠️ Limited; most loading is static (at compile time)     |

---
class: center, middle

Used in games (e.g., by Nintendo), routers, small devices

---
class: center, middle

### **Interactive Configuration or Scripting**

---
class: center, middle

Embedded devices that offer user customization or configuration may benefit from a high-level scripting language like Ruby.

---
class: center, middle

*Example:* A network device or appliance with Ruby scripts to configure settings.

---
class: center, middle

### **Testing and DevOps in Embedded**

---
class: center, middle

Useful for **test harnesses**, **firmware test frameworks**, or **log parsing**.

---
class: center, middle

### **Limitations and Tradeoffs** of using Ruby for embedded

---

- Performance in real-time, polling use-cases

- Memory usage

- Ecosystem Fit

---
class: center, middle

Interpreted Ruby (MRI) is large and slow compared to C/C++, Rust, or even Lua and Python.

---
class: center, middle

Standard Ruby (MRI) needs tens of MB of RAM, which is unsuitable for most MCUs.

---
class: center, middle

Even **mruby**, while light, is still heavier than Lua or C.

---
class: center, middle

Most embedded libraries, drivers, and vendor SDKs are written in C/C++.

---
class: center, middle

Ruby lacks ecosystem support for **hardware interaction** (I²C, SPI, UART, GPIO) unless you write C extensions or bindings manually.

---

| Scenario                | Why Ruby?                                                                    |
| ----------------------- | ---------------------------------------------------------------------------- |
| Embedded Linux devices  | Use full Ruby for scripting and UI logic (e.g., industrial panels, routers). |
| Scripting inside C apps | Embed mruby for dynamic behavior without recompilation.                      |
| Configuration UIs       | Ruby DSLs or Sinatra-based apps for internal device configuration.           |
| Testing & automation    | Ruby is great for firmware validation, simulation, and tooling.              |

---
class: center, middle

## Building Ruby with [Buildroot](https://buildroot.org/) / [Yocto](https://www.yoctoproject.org/)

---

| Goal                                  | Explanation                                                                                                                 |
| ------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| 🧩 **Minimal Custom Linux**           | Both Buildroot and Yocto help you create a **lean, custom Linux OS** with just enough to run your Ruby application.         |
| 📦 **Bundled Ruby Runtime**           | You can cross-compile Ruby and bundle it directly into the root filesystem. No need for external dependencies at runtime.   |
| 🛠️ **Cross-compilation Support**     | Ruby is built natively for your target hardware (e.g., ARM, RISC-V), not your dev machine. These tools handle that cleanly. |
| 🔐 **Security & Control**             | Smaller attack surface and deterministic builds—useful for appliances, routers, kiosks, etc., that need Ruby scripting.     |
| 🚀 **Faster Boot, Lower RAM/Storage** | You avoid bloat by skipping package managers, GUI libs, or unwanted features. Ideal for headless devices.                   |
| 🔁 **Reproducible Builds**            | Both systems allow **rebuilding the full image** from scratch, useful for versioned releases and CI pipelines.              |

---
class: center, middle

```bash
git clone https://github.com/buildroot/buildroot.git
```

---
class: center, middle

```bash
make list-defconfigs
```

---
class: center, middle

```bash
make menuconfig
```

---

### Enable Ruby under

```
# Target packages  --->
#   Interpreter languages  --->
#     [*] ruby
```

---
class: center, middle

```bash
make
```

---
class: center, middle

In the **Yocto Project**, **Poky** is the **reference distribution** and the **build system**.

---
class: center, middle

It's the central piece that combines all core Yocto components to let you build complete embedded Linux systems.

---

### Breakdown: What *Poky* Includes

Poky is a Git repository that bundles:

1. **BitBake** – the task executor and core build engine.
2. **meta/** – core metadata recipes for building Linux (e.g., GCC, systemd, BusyBox, etc.).
3. **meta-poky/** – reference distribution layer with configuration, image recipes, etc.
4. **meta-yocto-bsp/** – board support packages (BSPs) for reference hardware (e.g., QEMU).
5. Configuration templates (e.g., `local.conf`, `bblayers.conf`) to get you started.

---

### Purpose of Poky

- **Reference Implementation**: It shows how to structure a working Yocto project.

- **Starting Point**: You can clone Poky and then add your own layers (e.g., `meta-mycompany`) for your product.

- **Example Layers**: Shows how to use best practices with layers, configuration, and recipes.

---

### Structure Example (when you clone poky)

```bash
poky/
├── bitbake/             # BitBake engine
├── meta/                # Core metadata
├── meta-poky/           # Reference distribution configs
├── meta-yocto-bsp/      # BSPs for QEMU and other boards
├── oe-init-build-env    # Setup script for environment
└── scripts/             # Helper scripts
```

---
class: center, middle

> **Poky = BitBake + core metadata + sample config layers (meta-poky, BSPs)**

---
class: center, middle

> It's the **official Yocto reference build system** used to build and test Linux distros for embedded systems.

---
class: center, middle

```bash
git clone -b langdale https://git.yoctoproject.org/git/poky.git
```

---
class: center, middle

```bash
source oe-init-build-env
```

---

In `build/conf/local.conf`, add:

```bash
IMAGE_INSTALL:append = " ruby"
```

---
class: center, middle

```bash
bitbake core-image-minimal
```

This will take a while the first time (\~1–2 hours depending on CPU/network).

---
class: center, middle

```bash
runqemu qemuarm
```

---
class: center, middle

#### Possibilities with Yocto

---
class: center, middle

##### 1. **Reducing Ruby Interpreter Size**

---
class: center, middle

```bash
./configure \
  --disable-install-doc \
  --without-gmp \
  --disable-rpath \
  --with-out-ext=tk,sdbm,gdbm,win32,dbm \
  --enable-load-relative
```

---
class: center, middle

```bash
strip ruby
```

---
class: center, middle

##### 2. **Minimizing Gems**

---

🪓 Avoid:

- `bundler`

- Full Rubygems CLI (especially on embedded)

---

✅ Use **vendor mode**

Install gems on the host system, then **vendor** them:

```bash
bundle config set --local deployment true
bundle config set --local path vendor/bundle

bundle install
```

---
class: center, middle

Then copy only:

```
vendor/bundle/ruby/*/gems/<your-gems>
vendor/bundle/ruby/*/gems/<your-gems>/lib/
```

---
class: center, middle

##### 🪛 3. **Static Gems or Packaging Ruby Code**

---

- Place scripts in `/usr/share/myapp/*.rb`

- Write a small wrapper script (e.g., `/usr/bin/myapp`):

  ```bash
  #!/bin/sh
  exec ruby /usr/share/myapp/start.rb "$@"
  ```

---

✅ Convert into a single binary (optional):

Tools like [Travelling Ruby](https://github.com/phusion/traveling-ruby) or even [mruby](https://mruby.org/) let you ship small interpreters with embedded scripts, though **mruby lacks many gems and standard Ruby features**.

---
class: center, middle

##### 🔧 4. **Yocto Specific Tips**

---
class: center, middle

Ruby comes from `meta-oe` (bitbake `ruby`)

---

- To reduce size:

  - Use `.bbappend` to modify `do_install` and remove files

  - Strip binaries in `do_install_append()`

---

- To vendor gems:

  - Write a custom recipe for your app

  - Install your app into image via `IMAGE_INSTALL_append = " myrubyapp"`

---

### What to Keep vs. Strip

| Component         | Keep? | Why/Why Not                                |
| ----------------- | ----- | ------------------------------------------ |
| Core interpreter  | ✅     | Required                                   |
| Rubygems CLI      | ❌     | Avoid if not installing gems at runtime    |
| Bundler           | ❌     | Adds size; unnecessary in production       |
| Extension headers | ❌     | Strip post-build                           |
| Documentation     | ❌     | Use `--disable-install-doc`                |
| Stdlib (partial)  | ✅/⚠️  | Keep only required ones (e.g. `fileutils`) |

---

### Test for Unused Libs

After building, run in QEMU:

```bash
ldd `which ruby`
```

---

Check for:

- Unnecessary dynamic links (e.g. `libgmp`, `libreadline`)

- Strip or link statically if needed

---
class: center, middle

### Static vs Dynamic Linking

---
class: center, middle

#### 🧱 Definitions (Quick Recap)

---
class: center, middle

**Static Linking**: All libraries (e.g., `libc`, `libpthread`, `libm`, etc.) are compiled into the Ruby binary itself.

---

##### 🛠️ How (Buildroot Example)

```bash
make menuconfig
→ Toolchain
  → [*] Build static libraries
→ Target packages → Interpreter Languages → Ruby
→ Build Options → [*] Enable static build
```

---

Or patch Ruby’s `./configure`:

```bash
./configure LDFLAGS="-static"
```

---

You also need to:

```bash
export RUBY_STATIC=true
```

---
class: center, middle

Yocto static linking is harder (some recipes don't support `--enable-static` cleanly), but you can patch `ruby_%.bbappend` and tweak `EXTRA_OECONF`.

---
class: center, middle

**Dynamic Linking**: Ruby binary references shared libraries (`.so` files) at runtime, loaded from `/lib`, `/usr/lib`, etc.

---

##### 🛠️ How (default in Buildroot and Yocto)

Just build Ruby normally — it will link against shared libs like:

```bash
ldd `which ruby`
```

---

Example:

```
libm.so.6 => /lib/arm-linux-gnueabihf/libm.so.6
libpthread.so.0 => /lib/arm-linux-gnueabihf/libpthread.so.0
libc.so.6 => /lib/arm-linux-gnueabihf/libc.so.6
```

---

#### ⚖️ When to Use Which?

| Scenario                             | Static | Dynamic |
| ------------------------------------ | ------ | ------- |
| Minimal rootfs or BusyBox-only image | ✅      | ❌       |
| QEMU/real hardware with glibc/musl   | ✅/⚠️   | ✅       |
| You want to copy 1 binary and run    | ✅      | ❌       |
| Need smaller storage use overall     | ❌      | ✅       |
| You want flexibility to update       | ❌      | ✅       |
| You control the full rootfs layout   | ✅      | ✅       |

---

#### 🔍 Mixed Strategy (Hybrid)

You can:

- Statically link Ruby itself

- Dynamically link non-core tools (e.g., systemd, openssl)

- Use static Ruby with vendored gems (pure Ruby only, no native extensions)

---
class: center, middle

## Working with Files

---
class: center, middle

## Raw Sockets

---
class: center, middle

## Creating & managing processes

---

- `fork` - Creates a **child Ruby process**. Both parent and child continue execution.

- `exec` - **Replaces** the current Ruby process with an external command. **Never returns** if successful.

- `system` - Run a command, wait for it to finish. Returns: true if exit status is 0, false otherwise.

- `spawn` - Starts a **new external process**. Returns immediately with the PID. Ruby keeps running.

- Backticks ( `cmd` / `%x{}` ) - Runs a shell command and **returns its output** as a string. **Blocks until done**.

---

Can't use stdin with:

| Method                  | stdin support                                |
| ----------------------- | -------------------------------------------- |
| backticks               | ❌ No stdin control                          |
| `exec`                  | ❌ Replaces Ruby process; no stdin from Ruby |
| `system`                | 🚫 Limited — no direct stdin control         |

---
class: center, middle

### Ways to Pass to stdin

---
class: center, middle

`IO.popen` vs `spawn` vs `Open3.popen2`

---

| Method         | Can write to stdin?  | Can read output?  | Notes                         |
| -------------- | -------------------- | ----------------- | ----------------------------- |
| `IO.popen`     | ✅                   | ✅                | Simple and versatile          |
| `spawn` + pipe | ✅                   | ✅                | Needs manual pipe setup       |
| `Open3.popen2` | ✅                   | ✅                | Cleaner, safer subprocess API |
| backticks      | ❌                   | ✅                | Output only                   |
| `exec`         | ❌                   | ❌                | Replaces Ruby process         |

---
class: center, middle

## Blocks vs Procs vs Lambda

---
class: center, middle

A block is a chunk of code enclosed in {} or do...end, passed to a method implicitly.

---
class: center, middle

A Proc is an instance of the Proc class. It allows you to store a block in a variable.

---
class: center, middle

A lambda is a special kind of Proc with stricter rules.

---
class: center, middle

## Taking a closer look at variables in Ruby

---
class: center, middle

Ruby is pass by `?`

---
class: center, middle

Everything in Ruby is an object...

---
class: center, middle

...and variables hold references, not the actual objects.

---
class: center, middle

### Mutability

---
class: center, middle

Some Ruby objects are mutable (can change in place), others are immutable.

---

| Type                   | Mutable? |
| ---------------------- | -------- |
| `String`               | ✅ Yes    |
| `Array`                | ✅ Yes    |
| `Hash`                 | ✅ Yes    |
| `Integer`              | ❌ No     |
| `Symbol`               | ❌ No     |
| `true`, `false`, `nil` | ❌ No     |

---
class: center, middle

Enforcing mutability using: `.freeze`

---
class: center, middle

### What about cloning?

---
class: center, middle

`dup` vs `clone`

---

| Feature                   | `dup`            | `clone`                       |
| ------------------------- | ---------------- | ----------------------------- |
| Copies frozen state?      | ❌ No             | ✅ Yes                         |
| Copies singleton methods? | ❌ No             | ✅ Yes (in some Ruby versions) |
| Typical use               | Lightweight copy | Exact replica                 |

---
class: center, middle

Deep Copy?

---
class: center, middle

Ruby's standard library does not include a `deep_freeze` method.

---
class: center, middle

## Managing memory & object state

---
class: center, middle

Ruby’s heap, object lifecycle, and the ObjectSpace module all tie together to manage memory and object state behind the scenes.

---
class: center, middle

Ruby’s heap is not the system heap (`malloc`), but a custom managed heap of object slots (called `RVALUE`s).

---
class: center, middle

`ObjectSpace`

---

```ruby
ObjectSpace.count_objects
ObjectSpace.each_object
ObjectSpace.dump_all
```

---

| Method               | Purpose                         |
| -------------------- | ------------------------------- |
| `count_objects`      | Counts all objects by class     |
| `each_object(Class)` | Iterates over live objects      |
| `dump_all(output:)`  | Dumps heap as JSON for analysis |

---
class: center, middle

## GC

---

**Mark-and-Sweep** (classic strategy):

- *Mark phase*: Traverses object graph and marks reachable objects.
- *Sweep phase*: Frees unmarked (unreachable) objects.

---

**Generational GC** (since Ruby 2.1):

- **Young**: Newly created, short-lived objects.
- **Old**: Objects that survived multiple GC cycles.
- Optimizes for most objects dying young → fewer full GC cycles.

---

**Incremental GC** (since 2.2):

- Splits mark phase to reduce pause time.

---

**Compacting GC** (since Ruby 2.7, opt-in):

- Compacts live objects to reduce memory fragmentation.

---
class: center, middle

Garbage Compaction reorganizes live objects in memory to reduce fragmentation by placing them close together.

---
class: center, middle

This frees up larger contiguous blocks of memory and improves cache performance.

---
class: center, middle

Only works on *Movable* objects.

---

### Movable objects are Ruby heap-allocated objects that

- Have no external (C-level) pointers referencing them

- Are not frozen or pinned

- Are known to the GC as safe to relocate

When an object is movable, Ruby's GC can shift it to a different location in the heap to reduce fragmentation.

---

| Type                                        | Movable? | Notes                                              |
| ------------------------------------------- | -------- | -------------------------------------------------- |
| `String`                                    | ✅        | If not frozen or externally referenced             |
| `Array`                                     | ✅        | Elements are references — the container is movable |
| `Hash`                                      | ✅        | Keys/values are pointers — hash table can move     |
| Custom class instances                      | ✅        | Unless used by native extension                    |
| `Struct`                                    | ✅        | If fully Ruby-side                                 |
| Symbols                                     | ❌        | Internal, non-GC heap                              |
| `Fixnum`, `Float`, `true`, `nil`, `false`   | ❌        | Immediate values, not heap objects                 |
| Finalized objects                           | ❌        | Pinned during finalization                         |
| Objects referenced from native C extensions | ❌        | Automatically pinned for safety                    |

---

### What Makes an Object Unmovable (Pinned)?

- External C references:

  Native extensions like libxml, ffi, curses, etc., may store raw C pointers to Ruby objects.

  Ruby cannot track these safely → it disables compaction on such objects.

- Finalizers (`ObjectSpace.define_finalizer`):

  These may access internal memory structures → Ruby disables compaction.

- Frozen internal objects:

  Some internals (like method tables, constant names) are protected from relocation.

- Explicitly Pinned by C API:

  Extensions using `rb_gc_register_address()` pin objects manually.

---
class: center, middle

### 🔧 GC Controls (MRI)

---
class: center, middle

Ruby exposes many GC controls via the `GC` module.

---

```ruby
GC.start # Manually triggers GC
GC.disable # Disable automatic GC
GC.enable  # Enable it again
```

---
class: center, middle

```ruby
GC.stat    # Returns stats as a hash
```

---
class: center, middle

```ruby
GC.stat[:heap_live_slots] # Number of live (reachable) objects
GC.stat[:total_allocated_objects]
```

---

| Method                            | Purpose                                              |
| --------------------------------- | ---------------------------------------------------- |
| `GC.start`                        | Manually trigger GC                                  |
| `GC.disable`                      | Disable automatic GC                                 |
| `GC.enable`                       | Re-enable GC                                         |
| `GC.compact`                      | Compact the heap                                     |
| `GC.stat`                         | Return hash of GC stats                              |
| `GC.latest_gc_info`               | Info on the last GC                                  |
| `GC.verify_compaction_references` | Validate that object references remain valid (debug) |

---

### ⚙️ Advanced: GC in Embedded Ruby

If you’re using **Ruby in embedded systems (e.g., Yocto, Buildroot)**:

- Use **MRuby** for tighter control (no GC unless enabled).

- Tune memory behavior at compile time (via `GC_ARENA_SIZE`, etc.)

- Consider disabling compacting GC if memory is highly fragmented or constrained.

---
class: center, middle

### GC Tuning using Environment Variables

---
class: center, middle

set before Ruby starts

---
class: center, middle

```bash
export RUBY_GC_HEAP_INIT_SLOTS=100000
```

---

#### 🧱 Memory Allocation / Heap Growth

| Variable                              | Default       | Purpose                                   |
| ------------------------------------- | ------------- | ----------------------------------------- |
| `RUBY_GC_HEAP_INIT_SLOTS`             | 10000         | Initial object slot count                 |
| `RUBY_GC_HEAP_FREE_SLOTS`             | 4096          | GC triggers when fewer slots are free     |
| `RUBY_GC_HEAP_GROWTH_FACTOR`          | 1.8           | Controls heap growth rate                 |
| `RUBY_GC_HEAP_GROWTH_MAX_SLOTS`       | 0 (unlimited) | Caps growth per GC cycle                  |
| `RUBY_GC_HEAP_OLDOBJECT_LIMIT_FACTOR` | 2.0           | Triggers major GC if too many old objects |
| `RUBY_GC_HEAP_OLDOBJECT_LIMIT_MAX`    | 0             | Max limit of old objects before major GC  |

---

#### 🧬 Malloc / C-side Allocation Triggers

| Variable                             | Default | Purpose                            |
| ------------------------------------ | ------- | ---------------------------------- |
| `RUBY_GC_MALLOC_LIMIT`               | \~16MB  | GC if malloc’d memory exceeds this |
| `RUBY_GC_MALLOC_LIMIT_MAX`           | \~32MB  | Max limit for malloc trigger       |
| `RUBY_GC_MALLOC_LIMIT_GROWTH_FACTOR` | 1.4     | Controls malloc limit growth       |
| `RUBY_GC_OLDMALLOC_LIMIT`            | \~128MB | Similar, for old-gen memory        |

---

#### 🔄 Compaction (Ruby ≥ 2.7)

| Variable                | Default | Purpose                           |
| ----------------------- | ------- | --------------------------------- |
| `RUBY_GC_COMPACT`       | false   | Enable auto-compaction            |
| `RUBY_GC_COMPACT_STATS` | false   | Dump compaction stats             |
| `RUBY_GC_ENABLE_MOVE`   | true    | Allows compaction to move objects |

---
class: center, middle

## Debugging and Profiling

---
class: center, middle

### GC::Profiler

---

```ruby
GC::Profiler.enable
GC::Profiler.report
GC::Profiler.clear
```

---

| Feature               | Description                                |
| --------------------- | ------------------------------------------ |
| `GC::Profiler.enable` | Starts profiling GC runs                   |
| `report`              | Returns per-GC stats: time, type, duration |
| `raw_data`            | Array of hash per GC run                   |
| `clear`               | Clears collected data                      |

---

### 📈 Debugging / Profiling

| Variable          | Purpose                          |
| ----------------- | -------------------------------- |
| `RUBY_GC_PROFILE` | Enable GC::Profiler              |
| `RUBY_GC_DEBUG`   | Dump internal GC debug info      |
| `RUBY_GC_STRESS`  | Run GC after every alloc (slow!) |

---

#### Practical Tuning Setups for Low-Latency Embedded System

```bash
export RUBY_GC_HEAP_INIT_SLOTS=20000
export RUBY_GC_HEAP_GROWTH_FACTOR=1.2
export RUBY_GC_HEAP_GROWTH_MAX_SLOTS=10000
export RUBY_GC_MALLOC_LIMIT=8000000
export RUBY_GC_MALLOC_LIMIT_GROWTH_FACTOR=1.1
export RUBY_GC_COMPACT=false
```

---

### Memory issues in Ruby typically manifest as

- **Memory leaks** (objects not freed)
- **Excessive memory usage**
- **GC overhead**
- **Fragmentation**

---

Detecting memory leaks in **long-lived embedded Ruby applications**—such as those running in IoT or consumer electronics—is especially critical since they often:

- Run for **days/weeks without restart**

- Have **limited memory** (often under 512MB)

- Use **custom Ruby builds** or stripped-down interpreters (like MRuby, though standard CRuby is still common)

---
class: center, middle

#### Key Tools for Memory Profiling

---
class: center, middle

##### 🔹 `ObjectSpace`

---

```ruby
ObjectSpace.each_object(String).count
ObjectSpace.memsize_of(obj)   # Memory size in bytes
```

---

Check for Object Retention Patterns:

Leaks often come from:

| Cause                  | Example                       |
| ---------------------- | ----------------------------- |
| Global/class variables | `@@cache = []`                |
| Closures holding refs  | `-> { user }`                 |
| Circular references    | `A → B → A`                   |
| Long-lived threads     | `Thread.new { loop { ... } }` |
| C extension leaks      | mallocs not freed             |

---

**Use**:

```ruby
ObjectSpace.each_object(ClassName) do |obj|
  puts ObjectSpace.allocation_sourcefile(obj)
end
```

---
class: center, middle

##### 🔹 `GC.stat`

Use this to monitor garbage collector behavior.

---

```ruby
pp GC.stat
```

---
class: center, middle

##### Test under Simulated Runtime

Create a stress harness that mimics the embedded system’s long-term usage.

---

Example:

```ruby
100_000.times do
  MyService.handle_event(generate_fake_data)
  GC.start if rand < 0.1
end
```

---

Then:

- Monitor memory via `ps`, `top`, or `get_process_mem`

- Plot object counts with `ObjectSpace`

---
class: center, middle

##### Log GC Statistics Periodically

---

```ruby
loop do
  sleep 60
  puts GC.stat.slice(:heap_used, :heap_allocated_pages, :total_allocated_objects)
end
```

---
class: center, middle

Growing `heap_allocated_pages` = potential problem.

---
class: center, middle

##### 🔹 `memory_profiler`

Gem that provides a detailed report of memory allocations.

---
class: center, middle

**Purpose**: Analyze object allocations, retained memory, and garbage collection inefficiencies.

---
class: center, middle

```sh
gem install memory_profiler
```

---

```ruby
require 'memory_profiler'

report = MemoryProfiler.report do
  # code block to profile
end
report.pretty_print
```

---

**Key Features of memory_profiler**:

- Shows **retained objects** by type and source line.

- Helps detect **memory leaks** and **hotspots**.

- Lightweight, easy to integrate into test cycles.

---

**Limitations of memory_profiler**:

- No real-time profiling.

- Focused on Ruby-level allocations, not native memory.

---
class: center, middle

##### 🔹 `stackprof`

Sample-based profiler (CPU, object allocation).

---
class: center, middle

**Purpose**: Analyze **performance bottlenecks**, **memory allocations**, and **GC impact** with low overhead.

---
class: center, middle

```sh
gem install stackprof
```

---

```ruby
require 'stackprof'

StackProf.run(mode: :object, out: 'tmp.dump') do
  # code
end
```

---

Then visualize with:

```sh
stackprof tmp.dump
```

---

**Modes**:

- `:cpu` – samples execution stack every N microseconds.

- `:wall` – samples real time (good for I/O-heavy apps).

- `:object` – tracks object allocations.

---
class: center, middle

##### ⚙️ `ruby-prof`

Full Profiling Tool (Wall Time, Allocations, GC)

---
class: center, middle

**Purpose**: Instrument Ruby code for deep **performance and memory** insights.

---
class: center, middle

```sh
gem install ruby-prof
```

---

**Modes**:

- `ALLOCATIONS`

- `MEMORY`

- `WALL_TIME`, `PROCESS_TIME`

- `GC_RUNS`, `GC_TIME`

---

**Output Types**:

- Flat, Graph, Call Tree

- Callgrind (for `kcachegrind` GUI)

---

**Best For**:

- Profiling method-level memory use.

- Detecting high-GC or excessive allocation methods.

---
class: center, middle

##### 🧠 `valgrind` – Native Memory Profiler (for C/Ruby internals)

---
class: center, middle

**Purpose**: Track native memory leaks, invalid reads/writes, malloc/free mismatches.

---

**Use Case in Ruby**:

- Ruby's C extensions.

- Native segfaults or hard-to-debug memory bugs.

---
class: center, middle

```sh
valgrind ruby myscript.rb
```

---

**Gotchas**:

- Very slow (3–10x).

- Shows **native memory**, not Ruby object heap.

---

**Advanced**:

- Use with debugging symbols:

```sh
RUBY_DEBUG=yes ./configure --enable-debug-env
make && make install
valgrind --tool=memcheck ruby -d myscript.rb
```

---

| Option                    | Description                                                |
| ------------------------- | ---------------------------------------------------------- |
| `--leak-check=full`       | Detect and show all memory leaks                           |
| `--show-leak-kinds=all`   | Show definite, indirect, possible, reachable leaks         |
| `--track-origins=yes`     | Track origin of uninitialized values (slower, but helpful) |
| `--log-file=valgrind.log` | Save output to a log file                                  |

---
class: center, middle

##### 🔹 `derailed_benchmarks` (Optional)

Great for Rails apps. Helps detect where memory bloat starts.

---

```sh
bundle exec derailed bundle:mem
```

---
class: center, middle

##### 🔹 `objspace` stdlib (requires `RUBYOPT="--enable-frozen-string-literal"` for accuracy)

---

```ruby
require 'objspace'
ObjectSpace.trace_object_allocations do
  str = "hello"
  puts ObjectSpace.allocation_sourcefile(str)
end
```

---

| Tool              | Use Case                      | Tracks      | Overhead | Best For                     |
| ----------------- | ----------------------------- | ----------- | -------- | ---------------------------- |
| `memory_profiler` | Object allocation analysis    | Ruby heap   | Low      | Memory leaks, test profiling |
| `stackprof`       | Sampled profiler              | CPU, allocs | Very low | Realtime app profiling       |
| `ruby-prof`       | Full instrumentation profiler | All         | Medium   | Deep performance+memory dive |
| `valgrind`        | Native memory checker         | malloc/free | High     | C extensions, native bugs    |

---
class: center, middle

#### Common Techniques for debugging memory leaks

---

`1.` Check for leaks:

```ruby
GC.start
ObjectSpace.each_object(MyClass).count
```

Use this in a loop or long-running task to check if objects are accumulating.

`2.` Force GC and measure memory before/after:

```ruby
before = `ps -o rss= -p #{Process.pid}`.to_i
GC.start
after = `ps -o rss= -p #{Process.pid}`.to_i
puts "RSS change: #{before - after} KB"
```

---

`3.` Track allocations:

Use `ObjectSpace.trace_object_allocations` to pinpoint where objects are created.

`4.` Limit object retention:

- Avoid global variables, class variables, or caching unbounded data.
- Watch out for closures or procs holding references.

---

### 🐞 Special Notes for Embedded Context

`1.` **Strip it down**:

- Avoid heavy gems that retain global state

- Minimize metaprogramming and open classes

`2.` GC Tuning:

Use env vars or `GC::Profiler.enable` to adjust frequency.

```ruby
ENV['RUBY_GC_HEAP_OLDOBJECT_LIMIT_FACTOR'] = '1.0'
ENV['RUBY_GC_HEAP_INIT_SLOTS'] = '600000'
```

---

`3.` Consider Ruby version:

Ruby 3+ has **compacting GC**, which can help with fragmentation but may **introduce pauses**—test if worth enabling via:

```ruby
GC.verify_compaction_references(double_heap: true, toward: :empty)
GC.compact
```

---

#### Lightweight Native Monitoring

On constrained systems, avoid heavy tools:

- Use `/proc/<pid>/status` to track `VmRSS`, `VmSize`

- Use `valgrind` (only on dev machine) for extension/malloc issues

---

#### Fixing Tips

- **Use Symbols cautiously**: Avoid dynamic symbol generation (`:"#{input}"`) in older Ruby.

- **Use `GC.verify_compaction_references`** before compaction (if using compacting GC in 3.x+).

- **Avoid Object Retention**: Reuse where possible, or allow GC to clean.

---

#### Patterns That Cause Memory Leaks

| Pattern                      | Description                      |
| ---------------------------- | -------------------------------- |
| 🔁 Caches                    | LRU/memoization without eviction |
| 💣 Procs/lambdas             | Retain outer scope references    |
| 🔗 ActiveRecord              | Associations loading huge trees  |
| 🧵 Threads                   | Not GC-ed until joined           |
| 🧱 Large Arrays              | Holding unused large containers  |
| 🧙‍♂️ Singleton-like globals | Objects never released           |

---
class: center, middle

## 🔍 **Benchmarking Embedded Ruby Apps**

---

| Area      | Metrics                         | Tools / Methods                        |
| --------- | ------------------------------- | -------------------------------------- |
| CPU       | Execution time, CPU usage       | `Benchmark`, `time`, `perf`, `strace`  |
| Memory    | Peak usage, leaks, GC pressure  | `memory_profiler`, `/proc`, `valgrind` |
| I/O       | Disk/Socket throughput          | Custom timers, `iostat`, `strace`      |
| GC        | GC time, frequency, heap growth | `GC::Profiler`, `ObjectSpace`          |
| Threading | Context switches, lock time     | `htop`, `strace`, `dstat`, `rbspy`     |

---

### 🛠️ **Tools (All Embeddable)**

- **`Benchmark` (stdlib)** – Time small blocks of code.

- **`GC::Profiler`** – Analyze GC duration per cycle.

- **`ObjectSpace.each_object`** – Count object allocations.

- **`memory_profiler`** – Measure memory leaks/retention.

- **`rbspy`** – Sampling profiler, works without modifying code.

- **`valgrind` (native)** – Catch memory issues (ruby must be compiled with debug symbols).

- **`stackprof`** – For call graph and flamegraph data.

---
class: center, middle

`benchmark` (stdlib) vs `benchmark-ips` (gem)

---

| Feature              | `Benchmark` (stdlib)               | `benchmark-ips` (gem)                                |
| -------------------- | ---------------------------------- | ---------------------------------------------------- |
| Measures             | **Elapsed time** for a given run   | **Iterations per second** (IPS)                      |
| Accuracy             | Basic, affected by system variance | More robust via statistical sampling                 |
| Use case             | Small, deterministic operations    | Microbenchmarks, performance comparison              |
| Output               | Execution time (sec)               | Iterations/sec, error margin, comparison summary     |
| Dependencies         | ✅ None (stdlib)                    | ❌ Requires gem install (`gem install benchmark-ips`) |
| Embedded Suitability | ✅ Excellent (no dependencies)      | ⚠️ Needs Gem support (may not suit minimal Ruby)     |

---

When to Use What?

| Scenario                                      | Use                  |
| --------------------------------------------- | -------------------- |
| Embedded with minimal Ruby                    | `Benchmark` (stdlib) |
| Simple performance measurements               | `Benchmark`          |
| Comparing multiple implementations            | `benchmark-ips`      |
| Need statistically meaningful throughput info | `benchmark-ips`      |
| Measuring wall time for I/O                   | `Benchmark`          |

---
class: center, middle

## Concurrency in Ruby

---
class: center, middle

*Concurrency vs Parallelism*

---
class: center, middle

**Concurrency**: Handling multiple tasks at once — not necessarily at the same *time*, but switching between them.

---
class: center, middle

**Parallelism**: Running multiple tasks *simultaneously* on multiple CPU cores.

---
class: center, middle

MRI Ruby supports **concurrency** well but has limited **parallelism** due to the Global VM Lock (GVL).

---
class: center, middle

The GVL ensures that only one thread can execute Ruby code at a time in MRI, even if your machine has multiple CPU cores.

---

### 🔍 Why does the GVL exist?

1. **MRI is not thread-safe at the C level** — most Ruby objects are not protected with fine-grained locks.

2. The GVL simplifies memory management (especially for the garbage collector).

3. It avoids concurrency bugs at the interpreter level — at the cost of real parallelism.

---

### 🔬 MRI and GVL

- In **MRI**, the Global VM Lock means only one thread can execute Ruby code at a time.

- **I/O-bound** concurrency still works well (threads yield during I/O).

- For **parallel CPU-bound** tasks, prefer Ractors or switch to **JRuby**/**TruffleRuby**.

---

#### ✅ Alternatives to Get Around the GVL

1. **Ractors (Ruby 3.0+)**

   - Each Ractor has its own GVL.

2. **JRuby / TruffleRuby**

   - No GVL; threads run in parallel.

3. **Child processes / fork**

   - True OS-level concurrency.

4. **Native Extensions (C)**

   - Can release GVL around blocking code with `rb_thread_call_without_gvl`.

---
class: center, middle

### 🔧 **Concurrency Primitives in Ruby**

---
class: center, middle

#### **Threads** (native threads)

---
class: center, middle

Created via `Thread.new`

---
class: center, middle

In MRI (CRuby), threads are **real OS threads**, but only one can run Ruby code at a time due to the GVL.

---
class: center, middle

Useful for I/O-bound tasks.

---

##### ✅ GVL is released when...

MRI releases the GVL in certain **blocking operations**, like:

- File I/O (`read`, `write`)
- Network I/O (`Net::HTTP`, `TCPSocket`)
- Sleep (`sleep`, `IO#wait`, etc.)
- External C extensions that explicitly call `rb_thread_call_without_gvl`

This allows **I/O-bound threads to run concurrently**, even if not in parallel.

---
class: center, middle

##### ⚠️ Race Conditions

---
class: center, middle

A **race condition** happens when multiple threads access or modify shared data **without synchronization**.

---
class: center, middle

The operations are *not atomic* — even `counter += 1` is 3 steps (read, add, write).

---
class: center, middle

`Queue`, `SizedQueue`

---
class: center, middle

##### ⚠️ Deadlocks

---
class: center, middle

**Deadlock** = two or more threads wait on each other to release locks — and no one does.

---

💡 **Avoid deadlocks**:

- Lock in a consistent order.

- Use `Timeout.timeout` as a fallback.

- Prefer message-passing.

---

##### Summarising Threads

- Native OS threads.

- **Preemptive** — the VM switches between threads automatically.

- Works well for **I/O-bound** tasks in MRI, but **not true parallelism** due to the GVL.

---

Use When:

- You need concurrent network calls or disk I/O.

- You don't need fine-grained scheduling.

- Thread blocking is acceptable.

---
class: center, middle

#### **Fibers** (lightweight coroutines)

---

- Introduced in Ruby 1.9.

- Cooperatively scheduled: they yield control explicitly.

- Great for building your own schedulers or async frameworks.

---

- **Manual context switching** using `Fiber#resume` and `Fiber.yield`.

- Lightweight — no OS involvement.

- Not scheduled automatically — **you manage switching**.

---

- **Fibers share the same thread and memory**.

- No preemption: race conditions don’t occur unless you use multiple threads with fibers.

- **Fibers must yield manually** — so no interleaving surprises.

---

- Easy to share data

- No locks needed — unless used across threads

---

Use When:

- You want low-overhead concurrency.

- You're building an event loop or coroutine-style control.

---

*But:*

- Not scalable for parallelism

  Doesn't scale well without a scheduler or framework.

- You must yield often enough

  Tedious to manage manually.

---
class: center, middle

*Preemptive vs Cooperative Scheduling*

---
class: center, middle

**Preemptive Scheduling**: The system **interrupts** running tasks automatically to switch between them.

---
class: center, middle

OS or runtime forcibly switches tasks at intervals.

---
class: center, middle

**Cooperative Scheduling**: Tasks must **voluntarily yield** control to let others run.

---
class: center, middle

No context switch unless the code calls `yield`, `await`, etc.

---
class: center, middle

#### **Ractors** (Ruby 3.0+)

---
class: center, middle

Provide **true parallelism** by isolating memory

---
class: center, middle

No shared state — communication only via messages

---
class: center, middle

Ractors are Ruby’s take on the actor model

---

- Each actor (Ractor) has its own heap, stack, and **GVL**.

- Actors do not share memory.

- They communicate by message passing — like Erlang or Elixir.

---

`1.` **Each Ractor runs in its own thread**

- Internally, it’s a native thread (via pthreads or similar).

- Each Ractor has **its own Global VM Lock** — meaning they can execute Ruby code truly **in parallel**. Great for CPU-bound tasks.

---

`2.` **Memory Isolation**

- Objects created in one Ractor cannot be directly accessed by another.

- Only **shareable objects** or **deep copies** can be sent between ractors.

---

`3.` **Communication via Channels**

- Ractors send and receive messages using:

  - `r.send(obj)`
  - `Ractor.receive`
  - `r.take`
  - `Ractor.yield`

- Behind the scenes, Ruby uses **lock-free concurrent queues** to pass serialized or frozen objects across Ractor boundaries.

---

- There’s **no shared state**, so **no need for mutexes**.

- Message passing is **blocking by default**, like CSP (Go) or actors (Erlang).

---

##### What Happens on `r.send(obj)`?

1. Ruby checks if `obj` is **shareable** (e.g. symbol, integer, deeply frozen).

2. If yes → enqueue into ractor’s **incoming port**.

3. If no → raises `Ractor::IsolationError`.

---
class: center, middle

Ractor provides `.shareable?` and `.make_shareable(obj)` which deep freeze objects and ensure they’re usable across threads/ractors

---

##### 🔩 Internals of Ractor (MRI Implementation)

- Ractor is implemented in C inside `ractor.c` (MRI).

- Each Ractor wraps:

  - Its own **Ruby VM context** (`rb_vm_t`)
  - Its own **fiber scheduler**
  - Its own **GVL**

- Message passing uses:

  - `ractor_incoming_port_t` (a queue of incoming messages)
  - `ractor_message_t` (structure for passing frozen/shared Ruby values)

---

✅ You can convert an object to be shareable:

```ruby
obj = { a: 1, b: 2 }.freeze
Ractor.make_shareable(obj)
r.send(obj)
```

---

##### ⚠️ Restrictions by Design of Ractors

| Limitation                              | Why                          |
| --------------------------------------- | ---------------------------- |
| No global variables access              | To avoid hidden dependencies |
| No IO sharing                           | Avoid race conditions        |
| No mutable object sharing               | To enforce safety            |
| Cannot call methods on external objects | Isolation principle          |

---

##### Threads vs Ractors

| Feature            | Threads              | Ractors              |
| ------------------ | -------------------- | -------------------- |
| Memory model       | Shared               | Isolated             |
| Parallel execution | ❌ (GVL)              | ✅ (each has own GVL) |
| Communication      | Shared memory, mutex | Message passing only |
| Safety             | Unsafe without locks | Safe by design       |

---
class: center, middle

*Caveat:* Not all gems are Ractor-safe yet

---

##### 🔬 Debugging Ractor Execution

- Use `Ractor#inspect` to see internal state

- Enable verbose GC logs to see memory behavior

- Add `puts` statements to trace communication timing

---

##### Why Use Ractors?

| Feature               | Ractors                   |
| --------------------- | ------------------------- |
| CPU-bound parallelism | ✅ Yes                     |
| I/O-bound parallelism | Threads or `async` better |
| Shared memory         | ❌ Never                   |
| Actor-like isolation  | ✅ By default              |
| Race-free design      | ✅ Yes                     |

---
class: center, middle

#### EventMachine (Event Loop, Reactor Pattern)

.content-credits[https://www.rubydoc.info/gems/eventmachine/EventMachine]

---
class: center, middle

```bash
gem install eventmachine
```

.content-credits[https://github.com/eventmachine/eventmachine]

---

- Event-driven framework using **reactor pattern**.

- Based on a **single-threaded event loop**.

- Similar in style to Node.js.

---

*Use Cases:*

- You're building **high-throughput network servers** (TCP, HTTP, WebSocket).

- You want **non-blocking I/O** with callbacks.

---

*Pitfalls:*

- Callback hell.

- Hard to integrate blocking Ruby code or existing libraries.

---

##### 🔄 **Control Flow**

- `EM.run`: Starts the main event loop.

- `EM.stop`: Stops the event loop gracefully.

- `EM.defer`: Runs blocking code in a thread pool to avoid blocking the reactor.

---

##### 🔁 **Timers**

- `add_timer`: Runs a block once after a delay.

- `add_periodic_timer`: Repeats a block at fixed intervals.

---

##### 🔌 **TCP Server**

- `start_server`: Starts a TCP server that listens for incoming connections.

- `receive_data`: Callback triggered when the server receives data from a client.

- `send_data`: Sends data back to the connected client.

- `unbind`: Called when the client disconnects.

---

##### 📞 **TCP Client**

- `connect`: Connects to a remote TCP server asynchronously.

- `post_init`: Callback triggered when the connection is successfully established.

- `receive_data`: Called when the client receives a message from the server.

- `close_connection`: Gracefully closes the connection.

---

##### 🌐 **Async HTTP (with em-http-request)**

- `HttpRequest.get`: Makes a non-blocking HTTP GET request.

- `callback`: Runs when the HTTP request completes successfully.

- `errback`: Runs when the HTTP request fails.

---

##### 📡 **UDP Server**

- `open_datagram_socket`: Starts a UDP server to receive datagrams.

- `receive_data`: Triggered when a UDP packet is received.

---

| Use Case     | Module / Pattern                  |
| ------------ | --------------------------------- |
| Timers       | `add_timer`, `add_periodic_timer` |
| TCP Server   | `start_server` + module           |
| TCP Client   | `connect` + module                |
| HTTP (async) | `em-http-request` gem             |
| UDP server   | `open_datagram_socket`            |

---
class: center, middle

#### `async` Gem (Fiber-based Event Loop)

.content-credits[https://socketry.github.io/async/guides/getting-started/index]

---
class: center, middle

Asynchronous programming (in any language, including Ruby) allows running many things at the same time.

---
class: center, middle

Most often, those are multiple network I/O operations (like HTTP requests) because async is the most efficient at that.

---
class: center, middle

```bash
gem install async
```

.content-credits[https://github.com/socketry/async]

---
class: center, middle

Fiber-based concurrency using a scheduler.

---

- Modern, **structured concurrency** built on **Fibers + scheduler**.

- Replaces manual Fiber juggling.

- Like `async/await` in JS or Python.

---
class: center, middle

##### Async ecosystem

Async is also an ecosystem of gems that work nicely together.

.content-credits[https://brunosutic.com/blog/async-ruby]

---

- `async-http` a featureful HTTP client

- `falcon` HTTP server built around Async core

- `async-await` syntax sugar for Async

- `async-redis` Redis client

---
class: center, middle

... and many others

---
class: center, middle

While each of the gems provides something useful, the truth is you only need the core `async` gem to access most of its benefits.

---

*Use When:*

- You want a clean, modern, **async/await-like style** in Ruby.

- You're doing high-concurrency I/O like HTTP requests, DB access.

---

*Pitfalls:*

- Blocking operations **will block the whole reactor** unless handled.

- Requires you to use compatible (non-blocking) libraries.

---
class: center, middle

#### **`concurrent-ruby`**

Unified interface to threads, futures, actors, promises, etc.

---
class: center, middle

```bash
gem install concurrent-ruby
```

.content-credits[https://github.com/ruby-concurrency/concurrent-ruby]

---

class: center, middle

### Overview of concurrency

---

| Feature           | Threads    | Fibers       | EventMachine     | `async` Gem       |
| ----------------- | ---------- | ------------ | ---------------- | ----------------- |
| Scheduling        | Preemptive | Cooperative  | Event loop       | Fiber scheduler   |
| Parallelism (MRI) | ❌ (GVL)    | ❌            | ❌                | ❌                 |
| Suitable for      | I/O-bound  | Fine control | Network services | Structured I/O    |
| Blocking-safe?    | Mixed      | ❌            | ❌                | ❌ (must be async) |
| Easy to debug?    | Medium     | Hard         | Hard             | Easier            |
| Memory overhead   | Medium     | Low          | Low              | Low               |

---

| Model        | Sharing Allowed | How to Share                       | Race Safety                     |
| ------------ | --------------- | ---------------------------------- | ------------------------------- |
| Threads      | ✅ Yes           | Direct access, mutex               | ❌ Unsafe by default             |
| Fibers       | ✅ Yes           | Shared memory (same thread)        | ✅ Safe (unless using Threads)   |
| Ractors      | ❌ No            | `send`, `receive`, `take`, `yield` | ✅ Safe by design                |
| async gem    | ✅ Yes (Fibers)  | Shared memory in same thread       | ✅ Safe if I/O non-blocking      |
| EventMachine | ✅ Yes           | Same event loop/thread             | ✅ Safe unless manually threaded |

---

| Situation                             | Use...                        |
| ------------------------------------- | ----------------------------- |
| Parallel HTTP requests or file I/O    | **Threads** or `async`        |
| Custom coroutine logic                | **Fibers**                    |
| High-performance network server       | **EventMachine** or **async** |
| Async database calls, modern Ruby I/O | **async**                     |
| CPU-bound work                        | **Ractors** or processes      |

---

### 🔁 GVL in Practice: When it matters

| Task Type         | GVL Problem? | Solution                                     |
| ----------------- | ------------ | -------------------------------------------- |
| CPU-bound         | ❌ Yes        | Use **Ractors**, JRuby, or child processes   |
| I/O-bound         | ✅ No         | Threads work fine                            |
| Native extensions | ⚠️ Depends   | Must release GVL manually (e.g., `Nokogiri`) |
| Async I/O         | ✅ No         | Use `async`, EM, etc.                        |

---
class: center, middle

## Web Services

---
class: center, middle

### WEBrick: Embedded HTTP Server

---
class: center, middle

`WEBrick` is a pure-Ruby, single-threaded HTTP server that's great for development and small embedded use cases.

---
class: center, middle

```bash
gem install webrick
```

.content-credits[https://github.com/ruby/webrick]

---

#### 🔹 **Strengths**

- Zero dependencies (pure Ruby)

- Simple to embed in CLI tools or Ruby apps

- Great for local dev, diagnostics, mock APIs

- Works with Protobuf and custom handlers

---

#### 🔻 **Limitations**

- 🐌 **Single-threaded** by default — slow for concurrent clients

- 🧱 No HTTP/2, WebSocket, or streaming support

- 🛑 Lacks middleware, routing, async I/O

- 🧵 No native fiber or thread pooling

- 🚫 Not production-ready (no zero-downtime reloads, monitoring, etc.)

---
class: center, middle

Use `WEBrick` to **create** a basic embedded HTTP server.

---

#### ✅ **Use WEBrick When:**

- You’re building an **embedded control API**, dev server, or internal tool

- You want **zero setup** and **vanilla Ruby**

- You control all clients and traffic is low

---

### ❌ **Avoid WEBrick When:**

- You need **performance, concurrency, or real routing**

- You’re exposing the server to external/public clients

- You need **modern protocols** (HTTP/2, WebSockets)

---
class: center, middle

Use `Net::HTTP` to **call** web services (including your own WEBrick server).

---
class: center, middle

## Protobuf

.content-credits[https://protobuf.dev/]

---
class: center, middle

```bash
gem install google-protobuf
```

.content-credits[https://github.com/protocolbuffers/protobuf/tree/main/ruby]

---

### Steps

- Define Your `.proto`

  ```proto
  message Greet {
      string name = 1;
  }
  ```

- Compile to Ruby

  `protoc --ruby_out=. message.proto`

- Use in Ruby

  - `<Message>.new`
  - `<Message>.encode`
  - `<Message>.decode`

---
class: center, middle

```ruby
req['Content-Type'] = 'application/x-protobuf'
```

---

### JSON Interop

```ruby
json = message.to_h.to_json          # serialize to JSON
Message.new(JSON.parse(json)) # parse back
```

---

🔹 **Tips for Real-World Webserver with Protobuf**

- **Use gRPC** for bi-directional streaming and structured APIs (Ruby has a gRPC gem).

- **Wrap with `rack`** if you want middleware, without using Sinatra/Rails.

- **Use `puma` or `falcon`** for production-grade concurrency.

---
class: center, middle

## Metaprogramming

---
class: center, middle

Writing code that writes or manipulates other code at runtime.

---
class: center, middle

Ruby treats everything as an object, and class definitions are open and modifiable.

---

**Key concepts**:

- `send`, `public_send`
- `method_missing`, `respond_to_missing?`
- `define_method`, `class_eval`, `instance_eval`
- `const_get`, `const_set`
- `singleton_class`, `eigenclass`
- `Module#define_method`, `Module#included`

---
class: center, middle

### Core Tools for Metaprogramming

---

**Dynamic dispatch**:

  ```ruby
  obj.send(:method_name, *args)
  ```

---

**Intercepting calls**:

Intercepts calls to undefined methods and allows you to dynamically handle them.

  ```ruby
  def method_missing(name, *args); ...; end
  ```

*Use Case:* Dynamic attribute access

---

**Creating methods at runtime**:

Creates a method dynamically at **runtime**. Unlike `method_missing`, this defines a **real method**.

  ```ruby
  define_method(:greet) { puts "Hello!" }
  ```

*Use Case:* Creating accessors on the fly

---

**Runtime class manipulation**:

Used to evaluate code **within the context of a class** or *instance*, allowing you to define methods or constants dynamically.

  ```ruby
  class_eval do ... end
  instance_eval do ... end
  ```

*Use Case:* Defining methods from a config hash

---
class: center, middle

### Building Domain-Specific Languages (DSLs)

---
class: center, middle

DSLs allow users to describe behavior in a more natural or domain-specific syntax.

---

#### Types of DSLs

- **Internal DSLs** (most common in Ruby)

- External DSLs (custom parsers, rarely needed in Ruby)

---
class: center, middle

#### 🔹 Key techniques for DSLs

---

**Instance eval context switching**:

  ```ruby
  class Config
    def initialize(&block)
      instance_eval(&block)
    end
    def setting(name, value); ...; end
  end
  ```

---

**Block-based DSLs**:

  ```ruby
  Form.build do
    input :name
    submit "Save"
  end
  ```

---

**Fluent interfaces** (chaining):

  ```ruby
  class Query
    def where(...); self; end
    def order(...); self; end
  end
  ```

---

*Caveats and Best Practices*

- ✅ Use metaprogramming when it makes the code **simpler** for end users

- ❌ Avoid it if it makes debugging, documentation, or error messages confusing

- 🔍 Always document magic methods and behaviors

- 🧪 Test behavior, not implementation — use specs

---
class: center, middle

## Modular Architecture Blueprint for embedded ruby apps

---

### 📦 Example Directory Structure

```
/firmware
  ├── hal/              # C/C++ drivers
  ├── bindings/         # C extensions for Ruby
  ├── scripts/          # Ruby logic
  │   ├── config/
  │   ├── rules/
  │   ├── services/
  ├── main.c            # Entry point (initializes Ruby VM)
  ├── Makefile
```

---

🧱 **High-Level Architectural Layers**

`1.` **Hardware Abstraction Layer (HAL)**

- Native C/C++ layer
- Exposes APIs to Ruby via native extensions or FFI

`2.` **Ruby Native Interface**

- Interface between HAL and the embedded Ruby interpreter

`3.` **Scripting Core (Ruby VM)**

- Select based on constraints:

  - **CRuby** (MRI): requires embedded Linux
  - **MRuby**: most embedded-friendly, supports native compilation
  - **Artichoke**: alternative Rust-based Ruby for embedded
  - **Opal** (for JS-hosted embedded devices)

---

`4.` **Application Layer (User Code)**

- Business logic written in Ruby:

  - Rules, behavior scripts, finite state machines
  - Sensor event handling
  - Configurable tasks (via YAML/JSON/DSL)

---
class: center, middle

### Control Loop Pattern

---

Many embedded Ruby apps are structured as a control loop:

```ruby
loop do
  update_sensors
  compute_actions
  apply_outputs
  sleep interval
end
```

---

### 🧠 **Memory & GC Considerations**

- Avoid object churn in tight loops
- Preallocate buffers
- Use immutable objects
- Explicit `GC.disable` or `GC.start` at safe points
- For MRI:

  - Use `GC::Profiler` and `ObjectSpace`

- For MRuby:

  - Configure memory pools
  - No moving GC — more deterministic

---
class: center, middle

## SOLID Principles

---

- Single Responsibility

- Open/Closed

- Liskov Substitution

- Interface Segregation

- Dependency Inversion

---
class: center, middle

`1.` **Single Responsibility Principle (SRP)**

---
class: center, middle

A class should have only one reason to change...

---
class: center, middle

... meaning it should have only one job or responsibility.

---
class: center, middle

`2.` **Open/Closed Principle (OCP)**

---
class: center, middle

Software entities should be open for extension but closed for modification...

---
class: center, middle

... meaning new functionality should be added by adding new code, not by changing existing code.

---
class: center, middle

Use **Duck Typing** and **polymorphism** in Ruby to extend behavior without altering existing code.

---
class: center, middle

`3.` **Liskov Substitution Principle (LSP)**

---
class: center, middle

Subtypes must be substitutable for their base types.

---
class: center, middle

`i.e.)` Objects of a superclass should be replaceable with objects of its subclasses without altering the correctness of the program.

---
class: center, middle

In Ruby, this means **duck-typed classes** should honor expectations.

---
class: center, middle

`4.` **Interface Segregation Principle (ISP)**

---
class: center, middle

Clients should not be forced to depend on interfaces they do not use.

---
class: center, middle

This principle encourages the creation of fine-grained interfaces rather than large, all-encompassing ones.

---
class: center, middle

In Ruby, favor **smaller role-specific modules** over large monolithic ones.

---
class: center, middle

`5.` **Dependency Inversion Principle (DIP)**

---
class: center, middle

High-level modules should not depend on low-level modules. Both should depend on abstractions.

---
class: center, middle

Abstractions should not depend on details. Details should depend on abstractions.

---
class: center, middle

In Ruby, use **duck typing or dependency injection** to abstract implementations.

---

### SOLID Summary

| Principle | Key Idea              | Ruby Tip                                    |
| --------- | --------------------- | ------------------------------------------- |
| SRP       | One reason to change  | Split responsibilities into classes/modules |
| OCP       | Extend, don’t modify  | Use duck typing and polymorphism            |
| LSP       | Substitutability      | Maintain interface contracts                |
| ISP       | Specific over general | Compose with small modules                  |
| DIP       | Rely on abstractions  | Inject dependencies, not concrete classes   |

---
class: center, middle

## Refactoring

---

- Step 1: Identify Responsibilities

- Step 2: Refactor into Classes

- Step 3: Apply any design patterns to make the code extensible

---

### ✅ Benefits of Refactoring

| Procedural             | Refactored                      |
| ---------------------- | ------------------------------- |
| Hard to test           | Unit-testable                   |
| All logic in one place | SRP: separated responsibilities |
| No abstraction         | Extendable (e.g., JSON parser)  |
| Hard to debug          | Traceable through interfaces    |

---
class: center, middle

## Design Patterns

---
class: center, middle

Design patterns in Ruby offer elegant ways to solve recurring software design problems.

---
class: center, middle

Ruby’s dynamic and object-oriented nature allows for concise, readable implementations of classic design patterns.

---

| Pattern   | Purpose                             | When to Use                              |
| --------- | ----------------------------------- | ---------------------------------------- |
| Singleton | Single shared instance              | Global config, logger, device interface  |
| Strategy  | Swappable behavior/logic            | Varying protocols, retries, rendering    |
| Observer  | Broadcast changes to many listeners | Events, sensor updates, UI notifications |
| Reactor   | Non-blocking I/O/event manager      | Low-latency loops, embedded servers      |

---
class: center, middle

## Testing & Debugging

---
class: center, middle

Code
https://github.com/AgarwalConsulting/presentation-ruby-for-embedded-developers

Slides
https://ruby-for-embedded-developers.slides.AgarwalConsulting.com
