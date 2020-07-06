FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
      libc-dev gcc make \
      libgpm-dev libonig-dev libpng-dev libslang2-dev \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /build
COPY checksum checksum
ADD https://www.jedsoft.org/snapshots/jed-pre0.99.20-139.tar.gz jed.tar.gz
RUN sha256sum jed.tar.gz && sha256sum -c checksum
RUN mkdir jed && tar -C jed --strip-components 1 -xf jed.tar.gz
WORKDIR /build/jed
RUN ./configure
RUN make
RUN make install

FROM debian:buster-slim
COPY --from=build /usr/local/ /usr/local/
RUN apt-get update && apt-get -y --no-install-recommends install \
      libgpm2 libonig5 libpng16-16 libslang2 libslang2-modules slsh \
 && rm -rf /var/lib/apt/lists/*
CMD ["jed", "-e", "emacs"]
