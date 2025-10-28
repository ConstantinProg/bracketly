FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

COPY src/Bracketly.WebApp/Bracketly.WebApp.csproj Bracketly.WebApp/

RUN dotnet restore Bracketly.WebApp/Bracketly.WebApp.csproj

COPY src .
WORKDIR /src/Bracketly.WebApp

RUN dotnet build -c Release -o /app/build

FROM build AS publish
RUN dotnet publish -c Release -o /app/publish --no-restore /p:UseAppHost=false

FROM mcr.microsoft.com/dotnet/aspnet:8.0-alpine AS final

WORKDIR /app

COPY --from=publish /app/publish .

RUN adduser --disabled-password --home /app appuser && chown -R appuser /app
USER appuser

ENV ASPNETCORE_URLS=http://+:8080 \
    DOTNET_RUNNING_IN_CONTAINER=true \
    DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=true

EXPOSE 8080

ENTRYPOINT ["dotnet", "Bracketly.WebApp.dll"]
