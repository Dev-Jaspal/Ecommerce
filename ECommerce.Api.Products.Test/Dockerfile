#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/runtime:7.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["ECommerce.Api.Products.Test/ECommerce.Api.Products.Test.csproj", "ECommerce.Api.Products.Test/"]
COPY ["ECommerce.Api.Products/ECommerce.Api.Products.csproj", "ECommerce.Api.Products/"]
RUN dotnet restore "ECommerce.Api.Products.Test/ECommerce.Api.Products.Test.csproj"
COPY . .
WORKDIR "/src/ECommerce.Api.Products.Test"
RUN dotnet build "ECommerce.Api.Products.Test.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ECommerce.Api.Products.Test.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ECommerce.Api.Products.Test.dll"]
