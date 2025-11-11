FROM ghcr.io/fatedier/frps:v0.61.0

COPY frps.ini /etc/frp/frps.ini

ENTRYPOINT ["/usr/bin/frps"]
CMD ["-c", "/etc/frp/frps.ini"]
