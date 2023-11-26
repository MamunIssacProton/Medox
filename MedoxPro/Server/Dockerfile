#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["MedoxPro/Server/MedoxPro.Server.csproj", "MedoxPro/Server/"]
COPY ["MedoxPro/Client/MedoxPro.Client.csproj", "MedoxPro/Client/"]
COPY ["MedoxPro/Shared/MedoxPro.Shared.csproj", "MedoxPro/Shared/"]
RUN dotnet restore "MedoxPro/Server/MedoxPro.Server.csproj"
COPY . .
WORKDIR "/src/MedoxPro/Server"
RUN dotnet build "MedoxPro.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MedoxPro.Server.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MedoxPro.Server.dll"]
