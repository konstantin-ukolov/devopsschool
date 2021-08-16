FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build-env
WORKDIR /src

# Copy csproj and restore as distinct layers
COPY src/Cadastre3D.Web.Server.sln ./
COPY src/Cadastre3D.Web.Server/Cadastre3D.Web.Server.csproj ./Cadastre3D.Web.Server/
COPY src/Cadastre3D.Web.Contracts/Cadastre3D.Web.Contracts.csproj ./Cadastre3D.Web.Contracts/
RUN dotnet restore

# Copy everything else and build
COPY src/ .

RUN dotnet publish Cadastre3D.Web.Server.sln --no-restore -c Release -o out
RUN apt-get update
RUN apt-get install -y apt-utils
RUN apt-get install -y libgdiplus
RUN apt-get install -y libc6-dev 
RUN ln -s /usr/lib/libgdiplus.so/usr/lib/gdiplus.dll

# Build runtime image
FROM mcr.microsoft.com/dotnet/core/aspnet:2.2.5-stretch-slim
WORKDIR /app
COPY --from=build-env /src/Cadastre3D.Web.Server/out .
EXPOSE 80
ENTRYPOINT ["dotnet", "Cadastre3D.Web.Server.dll"]