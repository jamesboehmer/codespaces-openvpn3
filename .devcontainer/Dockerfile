# You can use any debian based image you want
ARG VARIANT=focal
# FROM mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}
FROM ubuntu:${VARIANT}
ARG VARIANT

# Install openvpn client
RUN export DEBIAN_FRONTEND=noninteractive && \
apt update && \
apt -y install apt-transport-https curl gnupg vim sudo && \
curl https://swupdate.openvpn.net/repos/openvpn-repo-pkg-key.pub > /usr/share/keyrings/openvpn-repo-pkg-key.pub && \
apt-key add /usr/share/keyrings/openvpn-repo-pkg-key.pub  && \
curl "https://swupdate.openvpn.net/community/openvpn3/repos/openvpn3-${VARIANT}.list" > /etc/apt/sources.list.d/openvpn3.list && \
apt update && \
apt -y install openvpn3=17~beta2+focal && \
mkdir -p /run/dbus && \
useradd -m vscode -s /bin/bash && \
echo "vscode  ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/vscode

COPY vpn /usr/local/bin/vpn
COPY init-dbus-daemon /usr/local/bin/init-dbus-daemon

