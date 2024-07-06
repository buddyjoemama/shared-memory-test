FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 5262

ENV ASPNETCORE_URLS=http://+:5262

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["shared-memory-reader/shared-memory-reader.csproj", "shared-memory-reader/"]
RUN dotnet restore "shared-memory-reader/shared-memory-reader.csproj"
COPY . .
WORKDIR "/src/shared-memory-reader"
RUN dotnet build "shared-memory-reader.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "shared-memory-reader.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "shared-memory-reader.dll"]
