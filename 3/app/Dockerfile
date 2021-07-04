FROM mcr.microsoft.com/dotnet/sdk:5.0-alpine

RUN mkdir /app

WORKDIR /app

RUN dotnet new webapi

EXPOSE 5000 5001

ENTRYPOINT ["dotnet run"]

