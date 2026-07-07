# Сборка статического бинарника без CGO.
FROM golang:1.23-alpine AS build
WORKDIR /src
COPY go.mod .
COPY main.go .
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o /proxy .

# distroless/static: ~2 МБ, есть CA-сертификаты (нужны для TLS к OpenAI), нет shell.
FROM gcr.io/distroless/static-debian12
COPY --from=build /proxy /proxy
# Промпт по умолчанию (в compose монтируется поверх для правок без пересборки).
COPY system_prompt.md /system_prompt.md
EXPOSE 4000
ENTRYPOINT ["/proxy"]
