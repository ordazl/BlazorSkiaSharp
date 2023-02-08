# Dockerfile
# specify the builder ("builder" can be any tag)
FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build

# Install global Dependencies
USER ContainerAdministrator
#RUN dotnet tool install -g dotnet-reportgenerator-globaltool
RUN dotnet workload install wasm-tools

WORKDIR /src
COPY ["BlazorSkiaSharp/Server/BlazorSkiaSharp.Server.csproj", "BlazorSkiaSharp/Server/"]
COPY ["BlazorSkiaSharp/Client/BlazorSkiaSharp.Client.csproj", "BlazorSkiaSharp/Client/"]
COPY ["BlazorSkiaSharp/Shared/BlazorSkiaSharp.Shared.csproj", "BlazorSkiaSharp/Shared/"]
RUN dotnet restore "BlazorSkiaSharp/Server/BlazorSkiaSharp.Server.csproj"
COPY . .
WORKDIR "/src/BlazorSkiaSharp/Server"
RUN dotnet build "BlazorSkiaSharp.Server.csproj" -c Debug -o /app/build

FROM build AS publish
RUN dotnet publish "BlazorSkiaSharp.Server.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazorSkiaSharp.Server.dll"]