FROM node:alpine

ENV LD_LIBRARY_PATH $LD_LIBRARY_PATH:/opt/calibre/lib
ENV PATH $PATH:/opt/calibre/bin
ENV CALIBRE_INSTALLER_SOURCE_CODE_URL https://raw.githubusercontent.com/kovidgoyal/calibre/master/setup/linux-installer.py
ENV GITBOOK_VERSION="3.2.3"

RUN apk update && \
    apk add --upgrade --no-cache \
    python python-dev python3 python3-dev \
    linux-headers build-base bash git ca-certificates \
    gcc mesa-gl qt5-qtbase-x11 wget xdg-utils xz && \
    rm -rf /var/cache/apk/*

RUN python3 -m ensurepip && \
    rm -r /usr/lib/python*/ensurepip && \
    pip3 install --upgrade pip setuptools && \
    if [ ! -e /usr/bin/pip ]; then ln -s pip3 /usr/bin/pip ; fi && \
    if [ -e /usr/bin/python ]; then rm -rf /usr/bin/python ; fi && \
    if [ ! -e /usr/bin/python ]; then ln -sf /usr/bin/python3 /usr/bin/python ; fi && \
    rm -r /root/.cache

RUN apk add --no-cache tzdata && \
    cp /usr/share/zoneinfo/Europe/Paris /etc/localtime && \
    echo "Europe/Paris" > /etc/timezone && \
    rm -rf /var/cache/apk/*

RUN wget -O- ${CALIBRE_INSTALLER_SOURCE_CODE_URL} | python2 -c "import sys; \
    main=lambda:sys.stderr.write('Download failed\n'); \
    exec(sys.stdin.read()); main(install_dir='/opt', isolated=True)" && \
    rm -rf /tmp/calibre-installer-cache

RUN npm install --global gitbook-cli && \
    gitbook fetch ${GITBOOK_VERSION} && \
    npm install --global markdownlint-cli && \
    npm cache clear --force

RUN pip3 --no-cache-dir install boto3 && \
    pip3 --no-cache-dir install awscli
