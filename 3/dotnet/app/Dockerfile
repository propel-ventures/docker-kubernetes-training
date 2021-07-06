FROM mcr.microsoft.com/dotnet/sdk:5.0-alpine

RUN mkdir /app

WORKDIR /app

ENV ASPNETCORE_URLS=http://*:5000

EXPOSE 5000

RUN dotnet new webapi --no-https

ENTRYPOINT ["dotnet", "run"]
