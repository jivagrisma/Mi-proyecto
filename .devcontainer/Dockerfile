# Use the official image as a parent image
FROM ubuntu:20.04

# Set the working directory
WORKDIR /usr/src/app

# Install dependencies
RUN apt-get update && apt-get install -y \
    curl \
    git \
    unzip \
    xz-utils \
    zip \
    libglu1-mesa \
    openjdk-8-jdk -y

# Set up the Android SDK
ENV ANDROID_SDK_ROOT /opt/android-sdk
RUN mkdir -p ${ANDROID_SDK_ROOT}/cmdline-tools/latest && \
    curl -o cmdline-tools.zip https://dl.google.com/android/repository/commandlinetools-linux-7302050_latest.zip && \
    unzip cmdline-tools.zip -d ${ANDROID_SDK_ROOT}/cmdline-tools/latest && \
    rm cmdline-tools.zip && \
    mv ${ANDROID_SDK_ROOT}/cmdline-tools/latest/cmdline-tools/* ${ANDROID_SDK_ROOT}/cmdline-tools/latest && \
    rm -r ${ANDROID_SDK_ROOT}/cmdline-tools/latest/cmdline-tools
ENV PATH ${PATH}:${ANDROID_SDK_ROOT}/cmdline-tools/latest/bin:${ANDROID_SDK_ROOT}/platform-tools

# Accept the license agreements of the SDK components
RUN yes | ${ANDROID_SDK_ROOT}/cmdline-tools/latest/bin/sdkmanager --licenses

# Install the SDK packages
RUN ${ANDROID_SDK_ROOT}/cmdline-tools/latest/bin/sdkmanager "build-tools;30.0.3" \
    "platforms;android-30" \
    "platform-tools"

# Set up the Flutter SDK
ENV FLUTTER_HOME /opt/flutter
RUN git clone https://github.com/flutter/flutter.git ${FLUTTER_HOME}
ENV PATH ${PATH}:${FLUTTER_HOME}/bin

# Enable Flutter web support
RUN flutter config --enable-web

# Run flutter doctor
RUN flutter doctor
