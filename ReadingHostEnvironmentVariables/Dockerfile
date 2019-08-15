FROM mcr.microsoft.com/dotnet/core/aspnet:2.2-stretch-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:2.2-stretch AS build
WORKDIR /src
COPY ["ReadingHostEnvironmentVariables.csproj", "ReadingHostEnvironmentVariables/"]
RUN dotnet restore "ReadingHostEnvironmentVariables/ReadingHostEnvironmentVariables.csproj"
COPY . .
ENV RamblingGeeksStreamURL="${RamblingGeekStreamURL}"
WORKDIR "/src/ReadingHostEnvironmentVariables"
RUN dotnet build "ReadingHostEnvironmentVariables.csproj" -c Release -o /app

FROM build AS publish
ENV RamblingGeeksStreamURL="${RamblingGeekStreamURL}"
RUN dotnet publish "ReadingHostEnvironmentVariables.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENV RamblingGeeksStreamURL="${RamblingGeekStreamURL}"
ENTRYPOINT ["dotnet", "ReadingHostEnvironmentVariables.dll"]