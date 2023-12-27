FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build-env

WORKDIR /dotnet

COPY . ./

RUN dotnet restore

RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/aspnet:7.0

WORKDIR /dotnet

COPY --from=build-env /dotnet/out .

ENTRYPOINT [ "dotnet", "Dotnet.Docker.dll"]
