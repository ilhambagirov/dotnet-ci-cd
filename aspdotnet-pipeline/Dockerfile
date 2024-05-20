FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 5003
EXPOSE 5003

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["aspdotnet-pipeline/aspdotnet-pipeline.csproj", "aspdotnet-pipeline/"]
RUN dotnet restore "aspdotnet-pipeline/aspdotnet-pipeline.csproj"
COPY . .
WORKDIR "/src/aspdotnet-pipeline"
RUN dotnet build "aspdotnet-pipeline.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "aspdotnet-pipeline.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "aspdotnet-pipeline.dll"]
