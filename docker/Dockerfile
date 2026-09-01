# =========================================================
# ROS 2 Humble - kaugõppe konteiner (TRO029)
#
# Lihtsustatud versioon algsest TRO029 Dockerfile'ist: puuduvad
# SSH-server, VNC/noVNC ja Jupyter, kuna need eeldasid pilvekeskkonda
# (GitHub Codespaces). Kohaliku Docker Desktopiga töötab tudeng
# otse `docker run -it ... bash` kaudu avatud shellis, GUI tööriistad
# (rviz2/rqt/turtlesim) on kasutatavad X11 forwardingu kaudu
# (vt peakorpuse README "Valikuline: graafiline liides").
# =========================================================
FROM ros:humble

SHELL ["/bin/bash", "-lc"]

RUN apt-get update && apt-get install -y --no-install-recommends \
    # --- ROS 2 CLI ---
    ros-humble-ros2cli \
    ros-humble-ros2launch \
    ros-humble-ros2topic \
    ros-humble-ros2node \
    ros-humble-ros2service \
    ros-humble-ros2param \
    \
    # --- ROS visual / demo (kasutatav X11 forwardinguga) ---
    ros-humble-rviz2 \
    ros-humble-rqt \
    ros-humble-rqt-common-plugins \
    ros-humble-turtlesim \
    ros-humble-demo-nodes-cpp \
    ros-humble-demo-nodes-py \
    \
    # --- Dev tööriistad ---
    python3-colcon-common-extensions \
    python3-pip \
    build-essential \
    cmake \
    git \
    vim \
    wget \
    \
    # --- Diagnostika ---
    iputils-ping \
    net-tools \
    \
    && rm -rf /var/lib/apt/lists/*

# ---------------------------------------------------------
# ROS keskkond käivitub automaatselt igas shellis (ei pea käsitsi
# `source /opt/ros/humble/setup.bash` tegema iga uue terminali korral)
# ---------------------------------------------------------
ENV ROS_DISTRO=humble
RUN echo 'source /opt/ros/${ROS_DISTRO}/setup.bash' >> /etc/bash.bashrc

# ---------------------------------------------------------
# Töökeskkond
# ---------------------------------------------------------
WORKDIR /workspace
RUN mkdir -p /workspace/ros2_ws/src
RUN echo 'source /workspace/ros2_ws/install/setup.bash 2>/dev/null || true' >> /etc/bash.bashrc

RUN printf '%s\n' \
      'echo ""' \
      'echo "╔════════════════════════════════════════════════════════════╗"' \
      'echo "║   TRO029 - ROS2 Sissejuhatus (kaugõppe konteiner)           ║"' \
      'echo "║   Tallinna Tehnikakõrgkool                                  ║"' \
      'echo "╚════════════════════════════════════════════════════════════╝"' \
      'echo "ROS2 keskkond on valmis! Töökaust: /workspace/ros2_ws"' \
      'echo ""' \
      >> /etc/bash.bashrc

CMD ["/bin/bash"]
