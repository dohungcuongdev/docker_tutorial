FROM busybox

ENV APP_BUILD=/usr/app/

COPY foo.txt $APP_BUILD/foo.txt
RUN echo 'foo' > /usr/app/foo.txt
RUN echo 'bar' >> /usr/app/foo.txt
