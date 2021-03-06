FROM swift:5.3

# Install SwiftLint
ENV SWIFTLINT_REVISION="0.40.3"
RUN git clone --branch $SWIFTLINT_REVISION https://github.com/realm/SwiftLint.git && \
    cd SwiftLint && \
    swift build --configuration release --static-swift-stdlib && \
    mv `swift build --configuration release --static-swift-stdlib --show-bin-path`/swiftlint /usr/bin && \
    cd .. && \
    rm -rf SwiftLint

# Install Git 2.28.0 (for GitHub Actions to locally clone repository)
# GitHub Actions would not locally clone repository
ENV GIT_REVISION="git-2.28.0"
RUN apt-get remove -y git && \
    apt-get update && \
    apt-get install -y curl && \
    apt-get install -y tcl-dev && \
    apt-get install -y gettext && \
    apt-get install -y libcurl4 libcurl4-openssl-dev && \
    curl -L https://www.kernel.org/pub/software/scm/git/$GIT_REVISION.tar.gz --output $GIT_REVISION.tar.gz && \
    tar -xf $GIT_REVISION.tar.gz && \
    cd $GIT_REVISION && \
    ./configure && \
    make && \
    make install && \
    cd .. && \
    rm -rf $GIT_REVISION && \
    rm -rf $GIT_REVISION.tar.gz

# Install Ruby & Bundler
ENV RUBY_REVISION="1:2.5.1"
ENV BUNDLER_REVISION="2.1.4"
RUN apt-get install -y ruby-full=$RUBY_REVISION && \
    gem install bundler:$BUNDLER_REVISION

# Install pronto gem

RUN apt-get -y update && apt-get install -y openssl curl git curl libssl-dev libreadline-dev zlib1g-dev autoconf bison build-essential libyaml-dev libreadline-dev libncurses5-dev libffi-dev libgdbm-dev libssl-dev cmake
RUN gem install pronto pronto-swiftlint

# Verify Installation & Version
RUN swiftlint version && \
    git version && \
    ruby --version && \
    bundler --version