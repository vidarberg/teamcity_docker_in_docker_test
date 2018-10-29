FROM microsoft/dotnet:2.1-aspnetcore-runtime AS base
WORKDIR /app
EXPOSE 80

FROM microsoft/dotnet:2.1-sdk AS build
WORKDIR /src
COPY ["DockerInDocker/DockerInDocker.csproj", "DockerInDocker/"]
RUN dotnet restore "DockerInDocker/DockerInDocker.csproj"
COPY . .
WORKDIR "/src/DockerInDocker"
RUN dotnet build "DockerInDocker.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "DockerInDocker.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "DockerInDocker.dll"]