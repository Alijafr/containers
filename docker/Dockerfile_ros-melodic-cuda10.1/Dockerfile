#
# ROS and Gazebo development environment
# based on px4's dockerfiles
#

FROM mzahana/base-ubuntu18-cuda10.1:latest
LABEL maintainer="Mohamed Abdelkader <mohamedashraf123@gmail.com>"

ENV DEBIAN_FRONTEND noninteractive
ENV ROS_DISTRO melodic
ARG TOKEN

RUN wget --quiet http://packages.osrfoundation.org/gazebo.key -O - | apt-key add - \
	&& sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -sc` main" > /etc/apt/sources.list.d/gazebo-stable.list' \
	&& apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654 \
	&& sh -c 'echo "deb http://packages.ros.org/ros/ubuntu `lsb_release -sc` main" > /etc/apt/sources.list.d/ros-latest.list' \
	&& sh -c 'echo "deb http://packages.ros.org/ros-shadow-fixed/ubuntu `lsb_release -sc` main" > /etc/apt/sources.list.d/ros-shadow.list' \
	&& apt-get update \
	&& apt-get -y upgrade \
	&& apt-get -y dist-upgrade \
	&& DEBIAN_FRONTEND=noninteractive apt-get -y --quiet --no-install-recommends install \
		ant \
		gazebo9 \
		gstreamer1.0-plugins-bad \
		gstreamer1.0-plugins-base \
		gstreamer1.0-plugins-good \
		gstreamer1.0-plugins-ugly \
		libeigen3-dev \
		libgazebo9-dev \
		libgstreamer-plugins-base1.0-dev \
		libimage-exiftool-perl \
		libopencv-dev \
		libxml2-utils \
		pkg-config \
		protobuf-compiler \
	  	geographiclib-tools \
		libeigen3-dev \
		libgeographic-dev \
		libopencv-dev \
		python-catkin-tools \
		python-tk \
		python3-pip \
		ros-$ROS_DISTRO-gazebo-ros-pkgs \
		ros-$ROS_DISTRO-mavlink \
		ros-$ROS_DISTRO-mavros \
		ros-$ROS_DISTRO-mavros-extras \
		ros-$ROS_DISTRO-pcl-conversions \
		ros-$ROS_DISTRO-pcl-msgs \
		ros-$ROS_DISTRO-pcl-ros \
		ros-$ROS_DISTRO-ros-base \
		ros-$ROS_DISTRO-rostest \
		ros-$ROS_DISTRO-rosunit \
		ros-$ROS_DISTRO-xacro \
		ros-$ROS_DISTRO-tf2-ros \
		ros-$ROS_DISTRO-roslint \
		ros-$ROS_DISTRO-rviz \
		xvfb \
		python-pil \
		python-yaml \
		libgeos-dev \
		python-lxml \
		libgdal-dev \
		build-essential \
		python-dev \
		libjpeg-dev \
		zlib1g-dev \
		libfreetype6-dev \
		python-virtualenv \
		python-rosdep \
	&& geographiclib-get-geoids egm96-5 \
	&& apt-get -y autoremove \
	&& apt-get clean autoclean \
	&& echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc \ 
	# pip
	&& pip install --upgrade numpy px4tools pymavlink \
	&& pip3 install --upgrade setuptools wheel \
	&& pip3 install --upgrade pyulog matplotlib \
	&& rm -rf /var/lib/apt/lists/{apt,dpkg,cache,log} /tmp/* /var/tmp/*

RUN echo "source /opt/ros/melodic/setup.bash" >> /home/arrow/.bashrc

RUN pip install Shapely Pillow MapProxy uwsgi
# ADD mapproxy.yaml /mapproxy.yaml

# install MAVLink headers
RUN git clone --depth 1 https://github.com/mavlink/c_library_v2.git /usr/local/include/mavlink/v2.0 && rm -rf /usr/local/include/mavlink/v2.0/.git 
RUN rosdep init && su -c "rosdep update -c /home/arrow/.ros/rosdep/sources.cache" - arrow

# Some QT-Apps/Gazebo don't not show controls without this
ENV QT_X11_NO_MITSHM 1

# Use UTF8 encoding in java tools (needed to compile jMAVSim)
ENV JAVA_TOOL_OPTIONS=-Dfile.encoding=UTF8

ENV ARROW_HOME /home/arrow
ENV CATKIN_WS $ARROW_HOME/catkin_ws

RUN echo "Working Folder is:$ARROW_HOME"

# RUN echo "Setting up the workspace"
RUN mkdir -p $CATKIN_WS/src
RUN echo "arrow" | sudo apt update

#ENTRYPOINT ["/usr/local/bin/ros_entrypoint.sh"]
