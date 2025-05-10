# Maintainer: Victor Queiroz <victorcqueirozg@gmail>

# PKGBUILD to build Mesa for the following on-board Intel GPU:
# 00:02.0 VGA compatible controller: Intel Corporation Alder Lake-S GT1 [UHD Graphics 730] (rev 0c) (prog-if 00 [VGA controller])
#   DeviceName: Onboard - Video
#   Subsystem: Lenovo Device 330b
#   Flags: bus master, fast devsel, latency 0, IRQ 143
#   Memory at 6000000000 (64-bit, non-prefetchable) [size=16M]
#   Memory at 4000000000 (64-bit, prefetchable) [size=256M]
#   I/O ports at 3000 [size=64]
#   Expansion ROM at 000c0000 [virtual] [disabled] [size=128K]
#   Capabilities: [40] Vendor Specific Information: Len=0c <?>
#   Capabilities: [70] Express Root Complex Integrated Endpoint, IntMsgNum 0
#   Capabilities: [ac] MSI: Enable+ Count=1/1 Maskable+ 64bit-
#   Capabilities: [d0] Power Management version 2
#   Capabilities: [100] Process Address Space ID (PASID)
#   Capabilities: [200] Address Translation Service (ATS)
#   Capabilities: [300] Page Request Interface (PRI)
#   Capabilities: [320] Single Root I/O Virtualization (SR-IOV)
#   Kernel driver in use: i915
#   Kernel modules: i915, xe

# See: https://gitlab.archlinux.org/archlinux/packaging/packages/mesa/-/blob/1dba0cd7ec1c4fd4152467696d1584885452fb81/PKGBUILD

pkgname=mesa-intel-uhd-730
pkgrel=1
pkgver=25.0.3
arch=('x86_64')
makedepends=(
  clang
  directx-headers
  expat
  gcc-libs
  glibc
  libdrm
  libelf
  libglvnd
  libpng
  libva
  libvdpau
  libx11
  libxcb
  libxext
  libxml2
  libxrandr
  libxshmfence
  libxxf86vm
  llvm
  llvm-libs
  lm_sensors
  rust
  spirv-llvm-translator
  spirv-tools
  systemd-libs
  vulkan-icd-loader
  wayland
  xcb-util-keysyms
  zlib
  zstd

  # shared between mesa and lib32-mesa
  cbindgen
  clang
  cmake
  elfutils
  glslang
  libclc
  meson
  python-mako
  python-packaging
  python-ply
  python-yaml
  rust-bindgen
  wayland-protocols
  xorgproto

  # valgrind deps
  valgrind

  # html-docs
  python-sphinx
  python-sphinx-hawkmoth
)

depends=(
  expat
  gcc-libs
  glibc
  libdrm
  libelf
  libglvnd
  libx11
  libxcb
  libxext
  libxshmfence
  libxxf86vm
  llvm-libs
  lm_sensors
  spirv-tools
  wayland
  zlib
  zstd
  libpng
  libva
)
provides=(
  'vulkan-mesa-layers'
  'opencl-driver'
  'opengl-driver'
  'vulkan-driver'
  'vulkan-intel'
  'vulkan-nouveau'
  'vulkan-radeon'
  'vulkan-swrast'
  'vulkan-virtio'
  'libva-mesa-driver'
  'mesa-vdpau'
  'mesa-libgl'
  'mesa'
)
conflicts=(
  'vulkan-mesa-layers'
  'opencl-clover-mesa'
  'vulkan-intel'
  'vulkan-nouveau'
  'vulkan-radeon'
  'vulkan-swrast'
  'vulkan-virtio'
  'libva-mesa-driver'
  'mesa-vdpau'
  'mesa-libgl'
  'mesa'
)

options=(
  # GCC 14 LTO causes segfault in LLVM under si_llvm_optimize_module
  # https://gitlab.freedesktop.org/mesa/mesa/-/issues/11140
  #
  # In general, upstream considers LTO to be broken until explicit notice.
  !lto
  # !debug
)
source=(
  "https://archive.mesa3d.org/mesa-${pkgver}.tar.xz"
  "https://archive.mesa3d.org/mesa-${pkgver}.tar.xz.sig"
  'lib64.txt'
)
sha512sums=(
  # mesa-$pkgver.tar.xz
  'a8ddfa3ac31869e82a49d14aaab0659d0496ae77db3f32aa0d5d28de8e1e4cace9fa652451a050fbc79281e8461cd70e86ad464aa387533387187fbcb604aaab'
  # mesa-$pkgver.tar.xz.sig
  '61e01bc666f4d502ee5ed1c317f5fab1c2ddff332bdb92cb4257fe3a12bcb484dd7880bf178f8ee0496ad7a87ef52bb8d46c6f48830dd77c50c9f8ee7459e9ed'
  # lib64.txt
  'f4596476b55798353297de3b78ea39280825d1c102776ee8891e2d4b95f3b6f6b69da93e019c53404e54b65e4420c0340c91f506b5528a654ad3a2b74e01c8f0'
)
validpgpkeys=(
  # Eric Engestrom <eric@engestrom.ch>
  '57551DE15B968F6341C248F68D8E31AFC32428A6'
  # Victor Queiroz (Personal PGP key) <victorcqueirozg@gmail.com>
  'EE92D3DE3C6C59543C0FF75CB7E510C142B88F4B'
)

# Rust crates for NVK, used as Meson subprojects
declare -A _crates=(
  equivalent 1.0.1
  hashbrown 0.14.1
  indexmap 2.2.6
  once_cell 1.8.0
  paste 1.0.14
  pest 2.7.11
  pest_derive 2.7.11
  pest_generator 2.7.11
  pest_meta 2.7.11
  proc-macro2 1.0.86
  quote 1.0.33
  roxmltree 0.20.0
  syn 2.0.68
  ucd-trie 0.1.6
  unicode-ident 1.0.12
)

for _crate in "${!_crates[@]}"; do
  _ver="${_crates[$_crate]}"
  source+=(
    "$_crate-$_ver.tar.gz::https://crates.io/api/v1/crates/$_crate/$_ver/download"
  )
  sha512sums+=(
    'SKIP'
  )
done

set_variables() {
  build_arch='x86_64'

  srcdir="$srcdir"
  srcdir_mesa="${srcdir}/mesa-$pkgver"
  meson_build_dir="${srcdir}/build"

  meson_configure_defs=(
    # Default library type for both_libraries
    'default_both_libraries=auto'

    # Default library type
    'default_library=shared'

    # Intel RT
    'intel-rt=enabled'

    # CLC
    # 'precomp-compiler=auto'
    'precomp-compiler=system'
    'mesa-clc=auto'
    'install-mesa-clc=true'
    'intel-clc=enabled'
    'install-intel-clc=true'
    # 'static-libclc=[]'
    # 'static-libclc=all'
    # 'install-precomp-compiler=true'
    'install-precomp-compiler=false'
    'microsoft-clc=disabled'

    # Vulkan
    'vulkan-drivers=intel,intel_hasvk,virtio,gfxstream,panfrost,freedreno'
    # 'vulkan-drivers=auto'
    #
    # Shader caching
    'shader-cache=enabled'
    'shader-cache-max-size=1G'
    #
    # Set Vulkan layers
    'vulkan-layers=device-select,intel-nullhw,overlay,screenshot,vram-report-limit'
    'gbm=enabled'

    # Kernel-mode drivers
    # 'freedreno-kmds=msm,kgsl,virtio,wsl'
    'freedreno-kmds=msm,kgsl,virtio'

    # GLVND
    'glvnd=enabled'

    # GLX
    'glx=auto'
    'glx-direct=true'
    'glx-read-only-text=false'

    # Tools
    'spirv-to-dxil=false'

    # LLVM
    'llvm=enabled'
    # Whether to link LLVM shared or statically.
    # 'shared-llvm=disabled'
    'shared-llvm=enabled'
    #
    # Whether to use LLVM for the Gallium draw module
    'draw-use-llvm=true'
    # 'draw-use-llvm=false'

    # AMD
    #
    # use experimental virtio backend for radeonsi/radv
    'amdgpu-virtio=false'
    #
    # LLVM
    'amd-use-llvm=false'

    # Debug
    #
    # Valgrind
    'valgrind=enabled'
    'libunwind=disabled'

    # Other
    'lmsensors=enabled'

    # Tests
    'build-tests=false'
    'enable-glcpp-tests=false'

    # Documentation
    #
    # HTML documentation
    'html-docs=disabled'

    # SELinux
    #
    # Deprecated
    # 'selinux=false'

    # Deprecated
    'osmesa=true'

    # OpenGL
    #
    # Whether to build a shared or static glapi.
    'shared-glapi=enabled'
    #
    # GLES
    # 'gles1=enabled'
    'gles1=disabled'
    'gles2=enabled'
    #
    # EGL
    'egl=enabled'
    #
    # Build support for desktop OpenGL.
    'opengl=true'

    # Compression
    'zlib=enabled'

    # Enable TensorFlow Lite delegate.
    'teflon=true'

    # Video codecs
    'video-codecs=all'

    # Optimizations
    'power8=disabled'
    'zstd=disabled'
    'datasources=[]'

    # Optional for Iris
    'intel-elk=true'

    # XML
    # 'Build custom xmlconfig (driconf) support. If disabled,
    # the default driconf file is hardcoded into Mesa. Requires expat.
    'xmlconfig=enabled'

    # VMWare
    #
    # vmware-mks-stats requires gallium VMware/svga driver.
    'vmware-mks-stats=true'
    # 'vmware-mks-stats=false'

    # X11, Xlib, etc.
    'xlib-lease=enabled'
    'legacy-x11=dri2'

    # Android
    #
    # Android Platform SDK version. Default: Nougat version.
    'platform-sdk-version=25'

    # Debug
    'split-debug=disabled'

    # Platforms
    'platforms=wayland,x11'

    # EGL
    #
    # the window system EGL assumes for EGL_DEFAULT_DISPLAY
    'egl-native-platform=auto'
    # 'egl-native-platform=wayland,x11,android,drm,surfaceless'
    # 'egl-native-platform=wayland,x11,drm,surfaceless'
    # 'egl-native-platform=wayland'

    # Android-reltaed
    # Build against android-stub
    'android-stub=false'
    'android-strict=false'
    # Use Android's libbacktrace
    'android-libbacktrace=disabled'

    # Gallium
    #
    # Drivers
    # 'gallium-drivers=iris,i915,zink,panfrost,virgl,svga,softpipe'
    'gallium-drivers=iris,i915,zink,panfrost,virgl,svga,softpipe,llvmpipe'
    #
    # Rust
    'gallium-rusticl=true'
    # 'gallium-drivers=iris,i915,zink,panfrost,softpipe,llvmpipe,virgl,svga'
    #
    # Enable VDPAU
    'gallium-vdpau=enabled'
    #
    # Enable VA
    'gallium-va=enabled'
    'gallium-rusticl-enable-drivers=auto'
    'gallium-xa=enabled'
    'gallium-nine=true'
    # 'gallium-opencl=icd'

    #
    # Enable HUD block/NIC I/O HUD status support.
    'gallium-extra-hud=false'
    #
    # build gallium d3d12 with graphics pipeline support.
    gallium-d3d12-graphics=disabled
    #
    # build gallium d3d12 with video support.
    'gallium-d3d12-video=disabled'
    #
    # build gallium D3D10 WDDM UMD frontend.
    'gallium-d3d10umd=false'
    #
    # build gallium d3d12 with video support.
    'gallium-d3d12-video=disabled'
    #
    # List of gallium drivers for which rusticl will be enabled by default
    'gallium-rusticl-enable-drivers=auto'
    #
    'unversion-libgallium=false'

    # expat
    'expat=auto'

    # Build options
    'default_library=shared'
    'prefix=/usr'
    'sysconfdir=/etc'
    'b_ndebug=true'
    'b_lto=true'
    'buildtype=release'
    'backend=ninja'
  )

  set_build_variables
}

set_LD_FLAGS() {
  # If no arguments are given, just stop here
  if ! [ $# -eq 0 ]; then
    for f in "$@"; do
      # Ignore if the string is empty
      if [ -z "$f" ]; then
        continue
      fi
      LDFLAGS="${LDFLAGS} $f"
    done
  fi

  export LDFLAGS
}

set_build_variables() {
  local compiler_flag_list
  compiler_flag_list=(
    '-Wfatal-errors'
  )

  MAKEFLAG="$MAKEFLAGS VERBOSE=1"
  export MAKEFLAGS

  # CFLAGS="$CFLAGS"
  # CXXFLAGS="$CXXFLAGS"
  # LDFLAGS="$LDFLAGS"
  unset CFLAGS
  unset CXXFLAGS
  unset LDFLAGS

  set_LD_FLAGS

  for f in "${compiler_flag_list[@]}"; do
    CFLAGS="${CFLAGS} $f"
  done
  export CFLAGS

  # Set CXXFLAGS
  CXXFLAGS="$CFLAGS $CXXFLAGS"
  export CXXFLAGS

  PKG_CONFIG_PATH=
  export PKG_CONFIG_PATH

  LD_PRELOAD=
  export LD_PRELOAD

  LD_LIBRARY_PATH=
  export LD_LIBRARY_PATH

  # MESON_PACKAGE_CACHE_DIR
  MESON_PACKAGE_CACHE_DIR="$srcdir/cache/meson"
  export MESON_PACKAGE_CACHE_DIR

  mkdir --parents --verbose "$MESON_PACKAGE_CACHE_DIR"
}

prepare() {
  set_variables

  cd "$srcdir_mesa" || exit 1

  local meson_setup_args
  meson_setup_args=()

  set_variables

  meson_configure_defs+=(
    # Link SPIR-V statically for 64-bit builds
    'static-libclc=spirv64'

    # CPU optimizations
    'sse2=true'

    # Tools
    'tools=glsl,panfrost,intel-ui'
  )

  meson_setup_args+=(
    --cross-file "$srcdir/lib64.txt"
  )

  set_LD_FLAGS "-m64"

  BINDGEN_EXTRA_CLANG_ARGS="-m64"

  # Add all definitions to the `meson_setup_args`
  for def in "${meson_configure_defs[@]}"; do
    meson_setup_args+=(
      '-D' "$def"
    )
  done

  # Export BINDGEN_EXTRA_CLANG_ARGS
  export BINDGEN_EXTRA_CLANG_ARGS

  meson setup "$meson_build_dir/$build_arch" "${meson_setup_args[@]}" || exit 1
}

build() {
  set_variables

  cd "$srcdir_mesa" || exit 1

  local meson_compile_args
  meson_compile_args=(
    -C "$meson_build_dir/$build_arch"
  )

  meson compile "${meson_compile_args[@]}" || exit 1
}

package() {
  set_variables

  mesa_arch_install x86_64
}

# The `set_variables` function must be called before calling this function
mesa_arch_install() {
  local builddir
  builddir="$meson_build_dir/$1"

  cd "$builddir" || exit 1

  local meson_install_args
  meson_install_args=(
    install
    -C "$builddir"
    --destdir "$pkgdir"
  )

  meson "${meson_install_args[@]}"
}
