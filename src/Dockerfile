FROM scratch
ADD alpine-minirootfs-3.11.2-x86_64.tar.gz /
ADD code-server2.1692-vsc1.39.2-alpine-x86_64.tar.gz /opt/
ADD doas-6.2p4.tar.gz /root/works/
COPY doas.conf /usr/local/etc/
RUN ln -s /opt/code-server*/code-server /usr/local/bin/ \
&& apk add --no-cache --virtual .code-server-deps \
libstdc++
WORKDIR /root/works/
RUN cd doas* \
&& apk add --no-cache --virtual .doas-build-deps \
bison \
gcc \
g++ \
linux-pam-dev \
make \
&& make -j $(nproc) \
&& make -j $(nproc) install \
&& apk del --purge .doas-build-deps \
&& cd .. \
&& rm -rf doas* \
&& chmod 600 /usr/local/etc/doas.conf \
&& apk add --no-cache --virtual .doas-deps \
linux-pam \
&& adduser -D user
USER user
WORKDIR /home/user/
EXPOSE 8080
ENTRYPOINT [ "code-server" ]