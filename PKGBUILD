pkgname=hsa-rocr
pkgver=7.0.1
pkgrel=2
pkgdesc="HSA Runtime API and runtime for ROCm"
arch=('x86_64')
url="https://github.com/ROCm/ROCR-Runtime"
license=('NCSA')
depends=(
    'rocm-core'
    'glibc'
    'gcc-libs'
    'numactl'
    'pciutils'
    'libelf'
    'libdrm'
    'rocm-device-libs'
    'rocprofiler-register'
)
makedepends=(
    'cmake'
    'rocm-llvm'
)
options=('!lto')
source=(https://github.com/ROCm/ROCR-Runtime/archive/rocm-${pkgver}/ROCR-Runtime-rocm-${pkgver}.tar.gz)
sha256sums=(f2004b98683a5b8e25d70034baee0f37a3b24dc1d4737b1c07b7830f4c10189d)

build() {
    cd ROCR-Runtime-rocm-${pkgver}

    # Silence warnings on optional libraries with -DNDEBUG,
    # https://github.com/RadeonOpenCompute/ROCR-Runtime/issues/89#issuecomment-613788944
    local cmake_args=(
        -B flarebird-build
        -D CMAKE_BUILD_TYPE=Release
        -D CMAKE_INSTALL_PREFIX=/usr
        -D CMAKE_INSTALL_LIBDIR=lib64
        -D BUILD_SHARED_LIBS=ON
        -D CMAKE_SHARED_LINKER_FLAGS=-ldrm_amdgpu
        -D INCLUDE_PATH_COMPATIBILITY=OFF
        -W no-dev
    )

    export CMAKE_PREFIX_PATH=/usr/lib64/rocm/llvm
    export HIP_DEVICE_LIB_PATH=/usr/lib64/amdgcn/bitcode

    cmake "${cmake_args[@]}"

    cmake --build flarebird-build
}

package() {
    cd ROCR-Runtime-rocm-${pkgver}

    DESTDIR=${pkgdir} cmake --install flarebird-build
}
