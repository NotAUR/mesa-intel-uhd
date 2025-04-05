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

pkgname=lib32-mesa-intel-uhd-730
pkgrel=1
pkgver=25.0.3
arch=('i686' 'x86_64')
makedepends=(
  lib32-clang
  lib32-directx-headers
  lib32-expat
  lib32-gcc-libs
  lib32-glibc
  lib32-libdrm
  lib32-libelf
  lib32-libglvnd
  lib32-libpng
  lib32-libva
  lib32-libvdpau
  lib32-libx11
  lib32-libxcb
  lib32-libxext
  lib32-libxml2
  lib32-libxrandr
  lib32-libxshmfence
  lib32-libxxf86vm
  lib32-llvm
  lib32-llvm-libs
  lib32-lm_sensors
  lib32-rust-libs
  lib32-spirv-llvm-translator
  lib32-spirv-tools
  lib32-systemd
  lib32-vulkan-icd-loader
  lib32-wayland
  lib32-xcb-util-keysyms
  lib32-zlib
  lib32-zstd
  lib32-libarchive

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

  # Other
  lua
)
# provides=(
#   mesa-intel
#   lib32-mesa
#   lib32-vulkan-virtio
#   lib32-mesa
#   lib32-vulkan-mesa-layers
# )
# replaces=(
#   mesa-intel
#   lib32-mesa
#   lib32-vulkan-virtio
#   lib32-mesa
#   lib32-vulkan-mesa-layers
# )
# overr
provides=(
  'lib32-vulkan-virtio'
  # 'lib32-vulkan-radeon'
  'lib32-mesa'
  'lib32-vulkan-intel'
  'lib32-vulkan-mesa-layers'
  'lib32-libva-mesa-driver'
  'lib32-mesa-vdpau'
  'lib32-mesa-libgl'
  'lib32-opengl-driver'
  'lib32-vulkan-driver'
)
replaces=(
  'lib32-vulkan-virtio'
  # 'lib32-vulkan-radeon'
  'lib32-mesa'
  'lib32-vulkan-intel'
  'lib32-vulkan-mesa-layers'
  'lib32-libva-mesa-driver'
  'lib32-mesa-vdpau'
  'lib32-mesa-libgl'
)
conflicts=(
  'lib32-vulkan-virtio'
  # 'lib32-vulkan-radeon'
  'lib32-mesa'
  'lib32-vulkan-intel'
  'lib32-vulkan-mesa-layers'
  'lib32-libva-mesa-driver'
  'lib32-mesa-vdpau'
  'lib32-mesa-libgl'
)
url="https://www.mesa3d.org"
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
  # "mesa::git+https://gitlab.freedesktop.org/mesa/mesa.git#tag=mesa-${pkgver}"
  'lib32.txt'
  '0001-gfxstream-Use-proper-log-format-for-32-bit-Vulkan.patch'
)
sha512sums=(
  # mesa-$pkgver.tar.xz
  'a8ddfa3ac31869e82a49d14aaab0659d0496ae77db3f32aa0d5d28de8e1e4cace9fa652451a050fbc79281e8461cd70e86ad464aa387533387187fbcb604aaab'
  # mesa-$pkgver.tar.xz.sig
  '61e01bc666f4d502ee5ed1c317f5fab1c2ddff332bdb92cb4257fe3a12bcb484dd7880bf178f8ee0496ad7a87ef52bb8d46c6f48830dd77c50c9f8ee7459e9ed'
  # lib32.txt
  'SKIP'
  # 0001-gfxstream-Use-proper-log-format-for-32-bit-Vulkan.patch
  '43cfb640d6a42074dbc87bbbbbc79cc71036165c874eb633b3927914a562fd51ff6bbef49adc3bef7031f75a111c6e4b3709e39fd2cff08c6c2e618b324374da'
 
)
validpgpkeys=(
  # Eric Engestrom <eric@engestrom.ch>
  '57551DE15B968F6341C248F68D8E31AFC32428A6'
  # Victor Queiroz (Personal PGP key) <victorcqueirozg@gmail.com>
  'EE92D3DE3C6C59543C0FF75CB7E510C142B88F4B'
)

# Rust crates for NVK, used as Meson subprojects
declare -A _crates=(
  equivalent      1.0.1
  hashbrown       0.14.1
  indexmap        2.2.6
  once_cell       1.8.0
  paste           1.0.14
  pest            2.7.11
  pest_derive     2.7.11
  pest_generator  2.7.11
  pest_meta       2.7.11
  proc-macro2     1.0.86
  quote           1.0.33
  roxmltree       0.20.0
  syn             2.0.68
  ucd-trie        0.1.6
  unicode-ident   1.0.12
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
  srcdir="$srcdir"
  srcdir_mesa="${srcdir}/mesa-$pkgver"
  meson_build_dir="${srcdir}/build"
  build_arch='i686'

  # MESA_SHADER_CACHE_SIZE
  MESA_SHADER_CACHE_SIZE="${MESA_SHADER_CACHE_SIZE:-512M}"

  meson_configure_defs=(
    # Default library type for both_libraries
    # 'default_both_libraries=auto'

    # Default library type
    # 'default_library=shared'

    # Intel RT
    # 'intel-rt=enabled'

    # CLC
    # 'precomp-compiler=auto'
    # 'precomp-compiler=system'
    # 'mesa-clc=auto'
    # 'install-mesa-clc=true'
    # 'intel-clc=enabled'
    # 'install-intel-clc=true'
    # 'static-libclc=[]'
    # 'static-libclc=all'
    # 'install-precomp-compiler=true'
    # 'install-precomp-compiler=false'
    # 'microsoft-clc=disabled'
    #
    # Vulkan
    'vulkan-drivers=intel,intel_hasvk,virtio,gfxstream,panfrost,freedreno'
    #
    # Shader caching
    'shader-cache=enabled'
    "shader-cache-max-size=${MESA_SHADER_CACHE_SIZE}"
    #
    # Set Vulkan layers
    # 'vulkan-layers=device-select,intel-nullhw,overlay,screenshot,vram-report-limit'
    'vulkan-layers=[]'
    'gbm=enabled'

    # Kernel-mode drivers
    # 'freedreno-kmds=msm,kgsl,virtio,wsl'
    # 'freedreno-kmds=msm,kgsl,virtio'

    # GLVND
    'glvnd=enabled'
    #
    # GLX
    'glx=auto'
    'glx-direct=true'
    'glx-read-only-text=false'
    #
    # Tools
    #
    'spirv-to-dxil=false'

    # LLVM
    #
    'llvm=enabled'
    # Whether to link LLVM shared or statically.
    # 'shared-llvm=disabled'
    'shared-llvm=enabled'
    #
    # Whether to use LLVM for the Gallium draw module
    'draw-use-llvm=true'
    # 'draw-use-llvm=false'

    #  
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
    'valgrind=disabled'
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
    # 'intel-elk=false'

    # XML
    # 'Build custom xmlconfig (driconf) support. If disabled,
    # the default driconf file is hardcoded into Mesa. Requires expat.
    'xmlconfig=enabled'

    # VMWare
    #
    # vmware-mks-stats requires gallium VMware/svga driver.
    # 'vmware-mks-stats=true'
    #
    # Set to `false` to avoid the following errors:
    #
    # src/gallium/winsys/svga/drm/vmw_screen.c:72:78: error: format ‘%lu’ expects argument of type ‘long unsigned int’, but argument 4 has type ‘size_t’ {aka ‘unsigned int’} [-Werror=format=]
    #    72 |          fprintf(stderr, "%s encountered locked mksstat TLS entry at index %lu.\n", __func__, i);
    #       |                                                                            ~~^                ~
    #       |                                                                              |                |
    #       |                                                                              |                size_t {aka unsigned int}
    #       |                                                                              long unsigned int
    #       |                                                                            %u
    'vmware-mks-stats=false'

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
    # 'default_library=shared'
    # 'prefix=/usr'
    # 'sysconfdir=/etc'
    # 'backend=ninja'
    'b_ndebug=true'
    'b_lto=true'
    # 'buildtype=release'
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
  MESON_PACKAGE_CACHE_DIR="$srcdir/cache/meson/i686"
  export MESON_PACKAGE_CACHE_DIR

  mkdir --parents --verbose "$MESON_PACKAGE_CACHE_DIR"
}

prepare() {
  set_variables

  cd "$srcdir_mesa" || exit 1

  # Apply patches
  local patch_file_list
  patch_file_list=(
    '0001-gfxstream-Use-proper-log-format-for-32-bit-Vulkan.patch'
  )

  for patch in "${patch_file_list[@]}"; do
    local patch_args
    patch_args=(
      --verbose
      -Np1 -i "$srcdir/$patch"
    )
    patch "${patch_args[@]}"
  done

  local meson_setup_args

  meson_setup_args=(
    # "$meson_build_dir/$build_arch"
    # --wrap-mode=nofallback
    # --reconfigure
  )

  meson_configure_defs+=(
    # CLC
    #
    # Link SPIR-V statically for 64-bit builds
    # 'static-libclc=[]'

    # CPU optimizations
    #
    # SSE2
    # Disable SSE2 for 32-bit builds
    # 'sse2=false'
    # 'sse2=true'

    # Do not build tools
    'tools=[]'
  )
  meson_setup_args+=(
    --cross-file "$srcdir/lib32.txt"
  )

  #set_LD_FLAGS "-Wl,-z,noexecstack"

  # Add all definitions to the `meson_setup_args`
  for def in "${meson_configure_defs[@]}"; do
    meson_setup_args+=(
      '-D' "$def"
    )
  done

  printf 'Setting up with arguments: %s\n' "${meson_setup_args[*]}"

  # Export BINDGEN_EXTRA_CLANG_ARGS
  BINDGEN_EXTRA_CLANG_ARGS="-m32"
  export BINDGEN_EXTRA_CLANG_ARGS

  # Inject subproject packages
  export MESON_PACKAGE_CACHE_DIR="$srcdir"

  arch-meson "$meson_build_dir/$build_arch" "${meson_setup_args[@]}" || exit 1
}

build() {
  set_variables

  local meson_compile_args

  meson_compile_args=(
    -C "$meson_build_dir/$build_arch"
  )

  meson compile "${meson_compile_args[@]}" || exit 1
}

package() {
  set_variables

  mesa_arch_install i686

  local unrelated_file_list
  unrelated_file_list=(
    /etc/OpenCL/vendors/rusticl.icd
    /usr/bin/mesa-overlay-control.py
    /usr/bin/mesa-screenshot-control.py
    /usr/include/EGL/eglext_angle.h
    /usr/include/EGL/eglmesaext.h
    /usr/include/GL/internal/dri_interface.h
    /usr/include/GL/osmesa.h
    /usr/include/d3dadapter/d3dadapter9.h
    /usr/include/d3dadapter/drm.h
    /usr/include/d3dadapter/present.h
    /usr/include/gbm.h
    /usr/include/xa_composite.h
    /usr/include/xa_context.h
    /usr/include/xa_tracker.h
    /usr/lib32/libvulkan_virtio.so
    /usr/share/drirc.d/00-mesa-defaults.conf
    /usr/share/glvnd/egl_vendor.d/50_mesa.json
    /usr/share/vulkan/explicit_layer.d/VkLayer_INTEL_nullhw.json
    /usr/share/vulkan/explicit_layer.d/VkLayer_MESA_overlay.json
    /usr/share/vulkan/explicit_layer.d/VkLayer_MESA_screenshot.json
    /usr/share/vulkan/explicit_layer.d/VkLayer_MESA_vram_report_limit.json
    /usr/share/vulkan/implicit_layer.d/VkLayer_MESA_device_select.json
  )

  for file in "${unrelated_file_list[@]}"; do
    rm --force --verbose "$pkgdir/$file"
  done
}

# The `set_variables` function must be called before calling this function
mesa_arch_install() {
  # If there is no first argument, throw
  if ! [ $# -eq 1 ]; then
    printf 'Error: mesa_arch_install requires 1 argument\n'
    exit 1
  fi

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
