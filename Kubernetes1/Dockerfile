FROM mcr.microsoft.com/dotnet/core/aspnet:3.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.0 AS build
WORKDIR /src
COPY ["Kubernetes1/Kubernetes1.csproj", "Kubernetes1/"]

RUN dotnet restore "Kubernetes1/Kubernetes1.csproj"
COPY . .
WORKDIR "/src/Kubernetes1"
RUN apt-get update -yq && apt-get upgrade -yq && apt-get install -yq curl git nano
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - && apt-get install -yq nodejs build-essential
RUN npm install -g npm
RUN npm install
RUN dotnet build "Kubernetes1.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Kubernetes1.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Kubernetes1.dll"]