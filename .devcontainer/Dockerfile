FROM ubuntu:20.04 as base
RUN  apt-get update \
  && apt-get install -y ca-certificates wget curl unzip


FROM base as downloader
RUN wget https://github.com/mmalekzadeh/motion-sense/raw/master/data/A_DeviceMotion_data.zip
RUN unzip A_DeviceMotion_data.zip


FROM base as python
ENV RYE_HOME="/opt/rye"
ENV PATH="$RYE_HOME/shims:$PATH"
ENV PYTHONUNBUFFERED True
SHELL [ "/bin/bash", "-o", "pipefail", "-c" ]
RUN curl -sSf https://rye.astral.sh/get | RYE_INSTALL_OPTION="--yes" bash && \
  rye config --set-bool behavior.global-python=true && \
  rye config --set-bool behavior.use-uv=true


FROM python as workspace
WORKDIR /workspace
COPY --from=downloader /A_DeviceMotion_data /tmp/motions