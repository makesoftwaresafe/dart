# Copyright 2021 Google LLC
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
RUN apt-get update && apt install -y g++-multilib git python curl

COPY . depot_tools
RUN rm -rf depot_tools/mayhem
ENV PATH="${SRC}/depot_tools:${PATH}"
RUN mkdir dart-sdk && cd dart-sdk && fetch dart
COPY mayhem/build.sh $SRC
COPY mayhem/patch.diff $SRC
WORKDIR $SRC/dart-sdk/sdk
# This is to fix Fuzz Introspector build by using LLVM old pass manager
# re https://github.com/ossf/fuzz-introspector/issues/305
ENV OLD_LLVMPASS 1

ENV FUZZING_LANGUAGE=c++ SANITIZER=address FUZZING_ENGINE=libfuzzer
RUN compile