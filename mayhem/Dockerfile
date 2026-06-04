# Copyright 2019 Google Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
################################################################################

FROM gcr.io/oss-fuzz-base/base-builder
RUN sed -i s/snapshot.ubuntu.com/archive.ubuntu.com/g /etc/apt/sources.list 2>/dev/null || true
RUN apt-get update --fix-missing && apt-get install -y --fix-missing make autoconf automake libtool curl
RUN git clone --depth 1 https://gitlab.xiph.org/xiph/ogg.git
COPY . libtheora
RUN rm -rf libtheora/mayhem
RUN git clone --depth 1 https://github.com/tmatth/oss-fuzz-fuzzers.git
RUN git clone --depth 1 https://github.com/fuzzamos/fuzzing-headers.git
RUN curl -sSL -o libtheora/m4/as-ac-expand.m4 https://gitlab.xiph.org/xiph/theora/-/raw/master/m4/as-ac-expand.m4 && \
    curl -sSL -o libtheora/m4/as-gcc-inline-assembly.m4 https://gitlab.xiph.org/xiph/theora/-/raw/master/m4/as-gcc-inline-assembly.m4 && \
    curl -sSL -o libtheora/m4/ogg.m4 https://gitlab.xiph.org/xiph/theora/-/raw/master/m4/ogg.m4 && \
    curl -sSL -o libtheora/m4/pkg.m4 https://gitlab.xiph.org/xiph/theora/-/raw/master/m4/pkg.m4 && \
    curl -sSL -o libtheora/m4/vorbis.m4 https://gitlab.xiph.org/xiph/theora/-/raw/master/m4/vorbis.m4
COPY mayhem/build.sh $SRC/

ENV FUZZING_LANGUAGE=c++ SANITIZER=address ARCHITECTURE=x86_64
RUN compile
RUN chmod +x /out/fuzzer-decoder
