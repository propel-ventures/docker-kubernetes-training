FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /app
COPY WeatherForecast /app
RUN dotnet build WeatherForecast.csproj -c Release -o /out

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS runtime
WORKDIR /app
COPY --from=build /out ./
EXPOSE 5000
ENTRYPOINT ["dotnet", "WeatherForecast.dll"]

