#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
RUN apt-get update && apt-get -y install libxml2 libgdiplus libc6-dev
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["apps/core/api/file-store/Devkit.FileStore/Devkit.FileStore.csproj", "apps/core/api/file-store/Devkit.FileStore/"]
COPY ["apps/core/api/communication/Devkit.Communication.FileStore/Devkit.Communication.FileStore.csproj", "apps/core/api/communication/Devkit.Communication.FileStore/"]
COPY ["apps/core/sdk/communication/Devkit.ServiceBus/Devkit.ServiceBus.csproj", "apps/core/sdk/communication/Devkit.ServiceBus/"]
COPY ["apps/core/sdk/webapi/Devkit.WebAPI/Devkit.WebAPI.csproj", "apps/core/sdk/webapi/Devkit.WebAPI/"]
COPY ["apps/core/sdk/metrics/Devkit.Metrics/Devkit.Metrics.csproj", "apps/core/sdk/metrics/Devkit.Metrics/"]
COPY ["apps/core/sdk/patterns/Devkit.Patterns/Devkit.Patterns.csproj", "apps/core/sdk/patterns/Devkit.Patterns/"]
COPY ["apps/core/sdk/data/Devkit.Data/Devkit.Data.csproj", "apps/core/sdk/data/Devkit.Data/"]
RUN dotnet restore "apps/core/api/file-store/Devkit.FileStore/Devkit.FileStore.csproj"
COPY . .
WORKDIR "/src/apps/core/api/file-store/Devkit.FileStore"
RUN dotnet build "Devkit.FileStore.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Devkit.FileStore.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Devkit.FileStore.dll"]