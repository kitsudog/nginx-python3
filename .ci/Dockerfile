FROM nginx:1.20-alpine as NGINX
FROM python:3.9 as PYTHON3
RUN pip install flask requests gevent && rm -rf /root/.cache/pip
FROM python:3.9-alpine
ENV LANG=C.UTF-8
RUN addgroup -g 101 -S nginx     && adduser -S -D -H -u 101 -h /var/cache/nginx -s /sbin/nologin -G nginx -g nginx nginx  
COPY --from=NGINX /etc/nginx /etc/nginx
COPY --from=NGINX /var/log/nginx /var/log/nginx
COPY --from=NGINX /usr/lib/ /usr/lib/
COPY --from=NGINX /lib/libz.so* /lib/libssl.so* /lib/libcrypto.so* /lib/
COPY --from=NGINX /usr/sbin/nginx /usr/sbin/nginx
COPY --from=NGINX /docker-entrypoint.sh /
COPY --from=NGINX /docker-entrypoint.d /docker-entrypoint.d
COPY --from=NGINX /var/cache/nginx /var/cache/nginx
COPY --from=NGINX /usr/share/nginx /usr/share/nginx
ENTRYPOINT ["/docker-entrypoint.sh"] 
CMD ["nginx","-g", "daemon off;"]
RUN apk add --no-cache uwsgi
COPY --from=PYTHON3 /usr/local/lib/python3.9/ /usr/local/lib/python3.9/
COPY --from=PYTHON3 /usr/local/bin/ /usr/local/bin/