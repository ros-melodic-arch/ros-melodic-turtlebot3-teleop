pkgdesc="ROS - Provides teleoperation using keyboard for Turtlebot3."
url='https://wiki.ros.org/turtlebot3_teleop'

pkgname='ros-melodic-turtlebot3-teleop'
pkgver='1.2.4'
arch=('any')
pkgrel=1
license=('Apache-2.0')

ros_makedepends=(
	ros-melodic-catkin
)

makedepends=(
	'cmake'
	'ros-build-tools'
	${ros_makedepends[@]}
)

ros_depends=(
    ros-melodic-rospy
	ros-melodic-geometry-msgs
)

depends=(
	${ros_depends[@]}
)

_dir="turtlebot3-${pkgver}/turtlebot3_teleop"
source=("${pkgname}-${pkgver}.tar.gz"::"https://github.com/ROBOTIS-GIT/turtlebot3/archive/${pkgver}.tar.gz"
	"python3-compatibility.patch"::"https://github.com/ROBOTIS-GIT/turtlebot3/commit/ddca617172a3549a3077682cd2cf7dc377616e01.diff")
sha256sums=('a75bd9e8f007cafcd3a0a7fd80a5428dfec69d13dcbd0a5bc018fac3154649fc'
            '67678a5af67a9cc2753235fbc78cc2cb57841b63884b5b35599ea32f382fcc89')

prepare() {
	cd ${srcdir}/turtlebot3-${pkgver}
	patch -p1 --input="${srcdir}/python3-compatibility.patch"
}

build() {
	# Use ROS environment variables.
	source /usr/share/ros-build-tools/clear-ros-env.sh
	[ -f /opt/ros/melodic/setup.bash ] && source /opt/ros/melodic/setup.bash

	# Create the build directory.
	[ -d ${srcdir}/build ] || mkdir ${srcdir}/build
	cd ${srcdir}/build

	# Fix Python2/Python3 conflicts.
	/usr/share/ros-build-tools/fix-python-scripts.sh -v 3 ${srcdir}/${_dir}

	# Build the project.
	cmake ${srcdir}/${_dir} \
		-DCMAKE_BUILD_TYPE=Release \
		-DCATKIN_BUILD_BINARY_PACKAGE=ON \
		-DCMAKE_INSTALL_PREFIX=/opt/ros/melodic \
		-DPYTHON_EXECUTABLE=/usr/bin/python3 \
		-DSETUPTOOLS_DEB_LAYOUT=OFF
	make
}

package() {
	cd "${srcdir}/build"
	make DESTDIR="${pkgdir}/" install
}
