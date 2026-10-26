FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
 
# Esta fase se usa para compilar el proyecto de servicio
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["SistemaOnline/SistemaOnline.csproj", "SistemaOnline/"]
RUN dotnet restore "./SistemaOnline/SistemaOnline.csproj"
COPY . .
WORKDIR "/src/SistemaOnline"
RUN dotnet build "./SistemaOnline.csproj" -c $BUILD_CONFIGURATION -o /app/build
 
# Esta fase se usa para publicar el proyecto que se copiará a la fase final
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./SistemaOnline.csproj" -c $BUILD_CONFIGURATION -o /app/publish \
    /p:UseAppHost=false
 
# Esta fase se usa en producción o cuando se ejecuta desde VS en modo normal
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "SistemaOnline.dll"]
