FROM quay.io/jupyter/minimal-notebook:python-3.12

USER root
RUN apt update && apt install -y sudo && \
  echo "jovyan ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/jovyan && \
  chmod 0440 /etc/sudoers.d/jovyan

RUN  apt update -qq && apt install -y -qq \
  apt-transport-https \
  build-essential \
  byobu \
  ca-certificates \
  curl \
  git \
  gnupg2 \
  locales \
  lsb-release \
  net-tools\
  software-properties-common \
  tmux \
  vim \
  wget 

ENV ROS_DISTRO=jazzy
ENV ROS_PATH=/opt/ros/${ROS_DISTRO}
ENV ROS_ROOT=${ROS_PATH}/share/ros
ENV ROS_WS=${HOME}/ros2_ws

RUN locale-gen en_US en_US.UTF-8 && \
  update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
ENV LANG=en_US.UTF-8

RUN add-apt-repository universe
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null
RUN apt update && \
  apt upgrade -y && \
  apt install -y \
  ros-dev-tools \
  ros-${ROS_DISTRO}-desktop \
  ros-${ROS_DISTRO}-nav2-msgs \
  ros-${ROS_DISTRO}-rosbridge-suite \
  ros-${ROS_DISTRO}-web-video-server && \
  apt clean && \
  echo "source ${ROS_PATH}/setup.bash" >> /home/jovyan/.bashrc && \
  echo "source ${ROS_WS}/install/setup.bash" >> /home/jovyan/.bashrc
RUN rosdep init && \
  rosdep update && \
  rosdep fix-permissions

USER jovyan
RUN pip install --upgrade \
  catkin_tools \
  cbor2 \
  colcon-common-extensions \
  ipywidgets \
  jupyterlab \
  jupyterlab-git \
  jupyter-remote-desktop-proxy\
  jupyter-resource-usage \
  jupyter-server-proxy \
  jupyter_offlinenotebook \
  lark \
  opencv-python \
  Pillow \
  pymongo \
  PySide2 \
  PyQt5 \
  rosdep \
  sidecar \
  && pip cache purge

# Avoid numpy version conflict due to vision_opencv issue:
# https://github.com/ros-perception/vision_opencv/issues/535
RUN pip install numpy==1.26.4

RUN mkdir -p ${HOME}/ros2_ws/src && \
  source ${ROS_PATH}/setup.bash && \
  git clone --branch master https://github.com/gstavrinos/ai_msc_robotics_assignments_ros ${HOME}/ros2_ws/src/ai_msc_robotics_assignments_ros && \
  cd ${HOME}/ros2_ws && \
  colcon build

WORKDIR ${HOME}/work
COPY --chown=jovyan:users ./ ${HOME}/work

COPY --chown=jovyan:users entrypoint.sh ${HOME}/
ENTRYPOINT ["/home/jovyan/entrypoint.sh"]
CMD [ "start-notebook.sh" ]

