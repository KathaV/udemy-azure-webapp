FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 7172

ENV ASPNETCORE_URLS=http://+:7172

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["Azure Web App Test/Azure Web App Test.csproj", "Azure Web App Test/"]
RUN dotnet restore "Azure Web App Test/Azure Web App Test.csproj"
COPY . .
WORKDIR "/src/Azure Web App Test"
RUN dotnet build "Azure Web App Test.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "Azure Web App Test.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Azure Web App Test.dll"]
