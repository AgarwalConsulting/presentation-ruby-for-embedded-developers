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

Code
https://github.com/AgarwalConsulting/presentation-ruby-for-embedded-developers

Slides
https://ruby-for-embedded-developers.slides.AgarwalConsulting.com
