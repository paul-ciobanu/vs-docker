#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["vs-docker/vs-docker.csproj", "vs-docker/"]
RUN dotnet restore "vs-docker/vs-docker.csproj"
COPY . .
WORKDIR "/src/vs-docker"
RUN dotnet build "vs-docker.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "vs-docker.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
EXPOSE 5201
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "vs-docker.dll"]
