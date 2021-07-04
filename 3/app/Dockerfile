FROM mcr.microsoft.com/dotnet/sdk:5.0-alpine

RUN mkdir /app

WORKDIR /app

RUN dotnet new webapi

ENTRYPOINT ["dotnet run"]

