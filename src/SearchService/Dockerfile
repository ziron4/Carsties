FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app
EXPOSE 80

# copy all .csproj files and restore as destinct layers.    Use of the same COPY command
# for every dockerfile in the project to take advantage of docker cashing
COPY Carsties.sln Carsties.sln
COPY src/AuctionService/AuctionService.csproj src/AuctionService/AuctionService.csproj 
COPY src/SearchService/SearchService.csproj src/SearchService/SearchService.csproj 
COPY src/GatewayService/GatewayService.csproj src/GatewayService/GatewayService.csproj 
COPY src/Contracts/Contracts.csproj src/Contracts/Contracts.csproj 
COPY src/IdentityService/IdentityService.csproj src/IdentityService/IdentityService.csproj 

# Restore package deps
RUN dotnet restore Carsties.sln

# Copy the app folders over
COPY src/SearchService src/SearchService
COPY src/Contracts src/Contracts
WORKDIR /app/src/SearchService
RUN dotnet publish -c Release -o /app/src/out

# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=build /app/src/out .
ENTRYPOINT [ "dotnet", "SearchService.dll" ]