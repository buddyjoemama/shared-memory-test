FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 5261

ENV ASPNETCORE_URLS=http://+:5261

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["shared-memory-server/shared-memory-server.csproj", "shared-memory-server/"]
RUN dotnet restore "shared-memory-server/shared-memory-server.csproj"
COPY . .
WORKDIR "/src/shared-memory-server"
RUN dotnet build "shared-memory-server.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "shared-memory-server.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "shared-memory-server.dll"]
