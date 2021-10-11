FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["dotnet-ci/dotnet-ci.csproj", "dotnet-ci/"]
RUN dotnet restore "dotnet-ci/dotnet-ci.csproj"
COPY . .
WORKDIR "/src/dotnet-ci"
RUN dotnet build "dotnet-ci.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "dotnet-ci.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "dotnet-ci.dll"]