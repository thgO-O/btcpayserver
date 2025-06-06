FROM mcr.microsoft.com/dotnet/sdk:8.0.404-bookworm-slim AS builder

RUN apt-get update && apt-get install -y --no-install-recommends chromium-driver \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /source
COPY nuget.config nuget.config
COPY Build/Common.csproj Build/Common.csproj
COPY BTCPayServer/BTCPayServer.csproj BTCPayServer/BTCPayServer.csproj
COPY BTCPayServer.Common/BTCPayServer.Common.csproj BTCPayServer.Common/BTCPayServer.Common.csproj
COPY BTCPayServer.Rating/BTCPayServer.Rating.csproj BTCPayServer.Rating/BTCPayServer.Rating.csproj
COPY BTCPayServer.Data/BTCPayServer.Data.csproj BTCPayServer.Data/BTCPayServer.Data.csproj
RUN cd BTCPayServer && dotnet restore
COPY BTCPayServer.Common/. BTCPayServer.Common/.
COPY BTCPayServer.Rating/. BTCPayServer.Rating/.
COPY BTCPayServer.Data/. BTCPayServer.Data/.
COPY BTCPayServer/. BTCPayServer/.
COPY Build/Version.csproj Build/Version.csproj

ENV SCREEN_HEIGHT 600 \
    SCREEN_WIDTH 1200

COPY . .

ARG CONFIGURATION_NAME=Release
RUN cd BTCPayServer.Tests && dotnet build --configuration ${CONFIGURATION_NAME} /p:CI_TESTS=true /p:RazorCompileOnBuild=true
WORKDIR /source/BTCPayServer.Tests
ENV CONFIGURATION_NAME=${CONFIGURATION_NAME}
ENTRYPOINT ["./docker-entrypoint.sh"]
