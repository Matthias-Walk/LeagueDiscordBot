FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["LeagueDiscordBot/LeagueDiscordBot.csproj", "LeagueDiscordBot/"]
RUN dotnet restore "LeagueDiscordBot/LeagueDiscordBot.csproj"
COPY . .
WORKDIR "/src/LeagueDiscordBot"
RUN dotnet build "LeagueDiscordBot.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "LeagueDiscordBot.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "LeagueDiscordBot.dll"]
