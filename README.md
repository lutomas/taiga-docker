## Starts

- Checkout
- `docker-compose -f taiga.yml up`

Should start without any exceptions. (2018-03-28)

## Docker-compose

For a complete taiga installation (``bartoszgadzala/taiga-back`` and ``bartoszgadzala/taiga-front``) you can use this docker-compose configuration:

```
db:
  image: postgres
  restart: always
  environment:
    - POSTGRES_USER=taiga
    - POSTGRES_PASSWORD=password
  volumes:
    - ./data/database:/var/lib/postgresql/data

taigaback:
  image: bartoszgadzala/taiga-back
  restart: always
  environment:
    - API_SCHEME=https
    - FRONT_SCHEME=https
    - HOSTNAME=taiga.example.com
    - SECRET_KEY=secret
    - PUBLIC_REGISTER_ENABLED=True
    - DEFAULT_FROM_EMAIL=taiga@example.com
    - EMAIL_USE_TLS=True
    - EMAIL_HOST=smtp.example.com
    - EMAIL_PORT=587
    - EMAIL_HOST_USER=user
    - EMAIL_HOST_PASSWORD=secret
  links:
    - db:postgres
  volumes:
    - ./data/taiga/back/media:/usr/local/taiga/media
    - ./data/taiga/back/static:/usr/local/taiga/static
    - ./data/taiga/back/logs:/usr/local/taiga/logs

taigafront:
  image: bartoszgadzala/taiga-front
  restart: always
  links:
    - taigaback
  ports:
    - 80:80
```
