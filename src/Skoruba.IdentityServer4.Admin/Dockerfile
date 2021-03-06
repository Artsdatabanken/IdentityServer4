FROM mcr.microsoft.com/dotnet/core/aspnet:2.2-stretch-slim AS base
WORKDIR /app
EXPOSE 9000

ENV ASPNETCORE_URLS="http://+:9000"
ENV ConnectionStrings__ConfigurationDbConnection "Server=[YOUR IP];Database=[YOUR DB];User ID=[YOUR USERNAME]; password=[YOUR PASSWORD];MultipleActiveResultSets=true"
ENV ConnectionStrings__PersistedGrantDbConnection "Server=[YOUR IP];Database=[YOUR DB];User ID=[YOUR USERNAME]; password=[YOUR PASSWORD];MultipleActiveResultSets=true"
ENV ConnectionStrings__IdentityDbConnection "Server=[YOUR IP];Database=[YOUR DB];User ID=[YOUR USERNAME]; password=[YOUR PASSWORD];MultipleActiveResultSets=true"
ENV ConnectionStrings__AdminLogDbConnection "Server=[YOUR IP];Database=[YOUR DB];User ID=[YOUR USERNAME]; password=[YOUR PASSWORD];MultipleActiveResultSets=true"
ENV Serilog__WriteTo__0__Args__connectionString "Server=[YOUR IP];Database=[YOUR DB];User ID=[YOUR USERNAME]; password=[YOUR PASSWORD];MultipleActiveResultSets=true"
ENV AdminConfiguration__IdentityServerBaseUrl "http://host.docker.internal:5000"

FROM mcr.microsoft.com/dotnet/core/sdk:2.2-stretch AS build
WORKDIR /src
COPY ["src/Skoruba.IdentityServer4.Admin/Skoruba.IdentityServer4.Admin.csproj", "src/Skoruba.IdentityServer4.Admin/"]
COPY ["src/Skoruba.IdentityServer4.Admin.BusinessLogic/Skoruba.IdentityServer4.Admin.BusinessLogic.csproj", "src/Skoruba.IdentityServer4.Admin.BusinessLogic/"]
COPY ["src/Skoruba.IdentityServer4.Admin.EntityFramework/Skoruba.IdentityServer4.Admin.EntityFramework.csproj", "src/Skoruba.IdentityServer4.Admin.EntityFramework/"]
COPY ["src/Skoruba.IdentityServer4.Admin.EntityFramework.Extensions/Skoruba.IdentityServer4.Admin.EntityFramework.Extensions.csproj", "src/Skoruba.IdentityServer4.Admin.EntityFramework.Extensions/"]
COPY ["src/Skoruba.IdentityServer4.Admin.BusinessLogic.Shared/Skoruba.IdentityServer4.Admin.BusinessLogic.Shared.csproj", "src/Skoruba.IdentityServer4.Admin.BusinessLogic.Shared/"]
COPY ["src/Skoruba.IdentityServer4.Admin.BusinessLogic.Identity/Skoruba.IdentityServer4.Admin.BusinessLogic.Identity.csproj", "src/Skoruba.IdentityServer4.Admin.BusinessLogic.Identity/"]
COPY ["src/Skoruba.IdentityServer4.Admin.EntityFramework.Identity/Skoruba.IdentityServer4.Admin.EntityFramework.Identity.csproj", "src/Skoruba.IdentityServer4.Admin.EntityFramework.Identity/"]
COPY ["src/Skoruba.IdentityServer4.Admin.EntityFramework.Shared/Skoruba.IdentityServer4.Admin.EntityFramework.Shared.csproj", "src/Skoruba.IdentityServer4.Admin.EntityFramework.Shared/"]
RUN dotnet restore "src/Skoruba.IdentityServer4.Admin/Skoruba.IdentityServer4.Admin.csproj"
COPY . .
WORKDIR "/src/src/Skoruba.IdentityServer4.Admin"
RUN dotnet build "Skoruba.IdentityServer4.Admin.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Skoruba.IdentityServer4.Admin.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Skoruba.IdentityServer4.Admin.dll"]