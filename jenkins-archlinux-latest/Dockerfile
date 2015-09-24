# This Dockerfile is used to build an image containing basic stuff to be used as a Jenkins slave build node.
FROM base/archlinux
MAINTAINER Teo Mrnjavac <teo@kde.org>

RUN pacman-key --populate
RUN pacman-key --refresh-keys

# Add archlinuxfr for yaourt.
RUN echo "[archlinuxfr]\n\
SigLevel = Never\n\
Server = http://repo.archlinux.fr/$arch\n" >> /etc/pacman.conf

# Make sure the package repositories are up to date.
RUN pacman -Syu --noconfirm

# Install OpenSSH, JDK 8, sudo and a bunch of other stuff for Calamares.
RUN pacman -S --noconfirm base-devel jdk8-openjdk cmake extra-cmake-modules \
                          qt5-base qt5-svg qt5-declarative qt5-quickcontrols \
                          qt5-graphicaleffects ki18n solid kcoreaddons python \
                          boost os-prober qt5-tools parted kconfig yaml-cpp \
                          libatasmart git polkit-qt5 openssh qt5-webkit kparts \
                          kservice konsole yaourt

RUN yaourt -S --noconfirm transifex-client-beta

# Add user jenkins to the image.
RUN useradd -m -s /bin/bash jenkins
# Set password for the jenkins user (you may want to alter this).
RUN echo "jenkins:jenkins" | chpasswd
# Allow the jenkins user to update the system.
RUN echo "jenkins ALL=(ALL) NOPASSWD: /usr/bin/pacman, /usr/bin/yaourt" > /etc/sudoers.d/jenkins

# Standard SSH port
EXPOSE 22

CMD ["/usr/bin/sshd", "-D"]
