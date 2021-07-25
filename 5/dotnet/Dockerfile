FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /app
COPY WeatherForecast/WeatherForecast.csproj .
RUN dotnet restore WeatherForecast.csproj
COPY ./WeatherForecast .
RUN dotnet build WeatherForecast.csproj -c Release -o /out

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS runtime
WORKDIR /app
COPY --from=build /out ./
ENTRYPOINT ["dotnet", "WeatherForecast.dll"]
