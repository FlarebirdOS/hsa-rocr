pkgname=hsa-rocr
pkgver=6.4.4
pkgrel=1
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
source=(https://github.com/ROCm/ROCR-Runtime/archive/rocm-${pkgver}/ROCR-Runtime-rocm-${pkgver}.tar.gz
    hsa-rocr-6.4-fix-missing-include.patch)
sha256sums=(bbc71e7f932ed218eca51a6ce9e437bb6c3118534d5be69bb957441bc3092a49
    6b7c62245fd9021ade8046e6a769e48c8c1868131dbac19531befc5f2a4c25b5)

prepare() {
    cd ROCR-Runtime-rocm-${pkgver}

    patch -Np1 < ${srcdir}/hsa-rocr-6.4-fix-missing-include.patch
}

build() {
    cd ROCR-Runtime-rocm-${pkgver}

    # Silence warnings on optional libraries with -DNDEBUG,
    # https://github.com/RadeonOpenCompute/ROCR-Runtime/issues/89#issuecomment-613788944
    local cmake_args=(
        -B flarebird-build
        -D CMAKE_BUILD_TYPE=Release
        -D CMAKE_INSTALL_PREFIX=/usr
        -D CMAKE_INSTALL_LIBDIR=lib64
        -D CMAKE_CXX_FLAGS="$CXXFLAGS -DNDEBUG"
        -D BUILD_SHARED_LIBS=ON
        -W no-dev
    )

    cmake "${cmake_args[@]}"

    cmake --build flarebird-build
}

package() {
    cd ROCR-Runtime-rocm-${pkgver}

    DESTDIR=${pkgdir} cmake --install flarebird-build
}
