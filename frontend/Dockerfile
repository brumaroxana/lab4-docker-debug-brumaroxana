FROM python:3.13.2-alpine3.21 AS build-image

WORKDIR '/app'

RUN apk add --no-cache linux-headers g++

COPY ./requirements.txt ./

RUN pip wheel --wheel-dir=/root/wheels -r requirements.txt

COPY ./app.py ./



FROM python:3.13.2-alpine3.21 AS production-image

WORKDIR '/app'


COPY --from=build-image /app/requirements.txt ./

COPY --from=build-image /app /app

RUN addgroup -S uwsgi && adduser -S uwsgi -G uwsgi

RUN pip install -r requirements.txt

COPY ./ ./

USER uwsgi

CMD ["python", "app.py"]
