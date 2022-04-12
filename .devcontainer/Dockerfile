# You can use any debian based image you want
ARG VARIANT=focal
FROM mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}
ARG VARIANT

# Install openvpn client
# RUN export DEBIAN_FRONTEND=noninteractive && \
# apt update && \
# apt -y install apt-transport-https curl gnupg apt-utils  && \
RUN export DEBIAN_FRONTEND=noninteractive && \
curl https://swupdate.openvpn.net/repos/openvpn-repo-pkg-key.pub > /usr/share/keyrings/openvpn-repo-pkg-key.pub && \
apt-key add /usr/share/keyrings/openvpn-repo-pkg-key.pub  && \
curl "https://swupdate.openvpn.net/community/openvpn3/repos/openvpn3-${VARIANT}.list" > /etc/apt/sources.list.d/openvpn3.list && \
apt update && \
apt -y install openvpn3=17~beta2+focal && \
mkdir -p /run/dbus

COPY vpn /usr/local/bin/vpn

