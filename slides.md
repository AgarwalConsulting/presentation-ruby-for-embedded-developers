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
class: center, middle

✅ Use **vendor mode**

Install gems on the host system, then **vendor** them:

```bash
bundle config set --local deployment true
bundle config set --local path vendor/bundle

bundle install
```

---

Then copy only:

```
vendor/bundle/ruby/*/gems/<your-gems>
vendor/bundle/ruby/*/gems/<your-gems>/lib/
```

---

class: center, middle

Code
https://github.com/AgarwalConsulting/presentation-ruby-for-embedded-developers

Slides
https://ruby-for-embedded-developers.slides.AgarwalConsulting.com
