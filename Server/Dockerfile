#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim AS build
WORKDIR /src
COPY ["Server/BlazorAppMono.Server.csproj", "Server/"]
COPY ["Shared/BlazorAppMono.Shared.csproj", "Shared/"]
COPY ["Client/BlazorAppMono.Client.csproj", "Client/"]
RUN dotnet restore "Server/BlazorAppMono.Server.csproj"
COPY . .
WORKDIR "/src/Server"
RUN dotnet build "BlazorAppMono.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BlazorAppMono.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazorAppMono.Server.dll"]