# Use pre-built multi-arch base image (includes px4_msgs and ROS2 dependencies)
FROM droneblocks/dexi-sim-ros2-base:humble-px4-1.16.0

WORKDIR /home/ubuntu

# Copy startup scripts (these can be updated without rebuilding the base)
COPY start-rosbridge.sh /usr/local/bin/start-rosbridge.sh
COPY start-dexi-bringup.sh /usr/local/bin/start-dexi-bringup.sh
RUN chmod +x /usr/local/bin/start-rosbridge.sh && \
    chmod +x /usr/local/bin/start-dexi-bringup.sh

# Auto-source dexi_ws if it exists (volume-mounted at runtime)
RUN echo '[ -f ~/dexi_ws/install/setup.bash ] && source ~/dexi_ws/install/setup.bash' >> /home/ubuntu/.bashrc

# Install NextDNS CLI for config c6a89a (started at runtime in start-dexi-bringup.sh)
RUN curl -sL https://repo.nextdns.io/nextdns.gpg \
        | sudo gpg --dearmor -o /usr/share/keyrings/nextdns.gpg && \
    echo "deb [signed-by=/usr/share/keyrings/nextdns.gpg] https://repo.nextdns.io/deb stable main" \
        | sudo tee /etc/apt/sources.list.d/nextdns.list && \
    sudo apt-get update && \
    sudo apt-get install -y nextdns && \
    sudo rm -rf /var/lib/apt/lists/*
