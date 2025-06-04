# Build stage
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /src

# Copy solution and project files
COPY ["src/Mobile.Bff.Shopping/Mobile.Bff.Shopping.csproj", "src/Mobile.Bff.Shopping/"]
COPY ["src/eShop.ServiceDefaults/eShop.ServiceDefaults.csproj", "src/eShop.ServiceDefaults/"]
COPY ["Directory.Build.props", "./"]
COPY ["Directory.Build.targets", "./"]
COPY ["Directory.Packages.props", "./"]

# Restore packages
RUN dotnet restore "src/Mobile.Bff.Shopping/Mobile.Bff.Shopping.csproj"

# Copy source code
COPY . .
WORKDIR "/src/src/Mobile.Bff.Shopping"
RUN dotnet build "Mobile.Bff.Shopping.csproj" -c Release -o /app/build

# Publish stage
FROM build AS publish
RUN dotnet publish "Mobile.Bff.Shopping.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Runtime stage
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS final
WORKDIR /app
EXPOSE 8080
EXPOSE 443
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Mobile.Bff.Shopping.dll"] 