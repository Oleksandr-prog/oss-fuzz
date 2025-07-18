# Copyright 2022 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
################################################################################

FROM gcr.io/oss-fuzz-base/base-builder-go
RUN apt-get update && apt-get install -y clang libbtrfs-dev pkg-config libdevmapper-dev
RUN git clone --depth 1 https://github.com/moby/moby
RUN git clone --depth 1 https://github.com/moby/go-archive
RUN git clone --depth 1 https://github.com/AdamKorcz/go-118-fuzz-build --branch=november-backup
COPY build.sh $SRC/
COPY *_fuzzer.go $SRC/

COPY *.options $SRC/

RUN wget https://sourceware.org/ftp/lvm2/LVM2.2.03.15.tgz \
    && tar -xvzf ./LVM2.2.03.15.tgz \
    && cd LVM2.2.03.15 \
    && ./configure --disable-selinux --enable-static_link \
    && make device-mapper

WORKDIR $SRC/moby
