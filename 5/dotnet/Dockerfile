FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /app
RUN dotnet new webapi -o WeatherForecast -f net5.0 --no-https
RUN dotnet build WeatherForecast/WeatherForecast.csproj -c Release -o /out

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS runtime
WORKDIR /app
COPY --from=build /out ./
ENTRYPOINT ["dotnet", "WeatherForecast.dll"]
