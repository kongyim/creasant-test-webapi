# creasant-test-webapi

## dockerize
```
docker build -t creasant-test-webapi .
```

## run image
```
docker run -d -p 8080:80 --name creasant-test-webapi creasant-test-webapi
```

## useful links 
1. [Create a Web API with ASP.NET Core and Visual Studio Code](https://docs.microsoft.com/en-us/aspnet/core/tutorials/web-api-vsc?view=aspnetcore-2.1)
1. [Dockerize a .NET Core application](https://docs.docker.com/engine/examples/dotnetcore/#create-a-dockerfile-for-an-aspnet-core-application)

## step by step creating this example

### create .NET webapi project
```
dotnet new webapi -o creasant-test-webapi --no-https
```

### create Dockerfile (./Dockerfile)
```
FROM microsoft/dotnet:sdk AS build-env
WORKDIR /app

# Copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore

# Copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out

# Build runtime image
FROM microsoft/dotnet:aspnetcore-runtime
WORKDIR /app
COPY --from=build-env /app/out .

ENTRYPOINT ["dotnet", "creasant-test-webapi.dll"]
```

### create docker ignore (./.dockerignore)
```
bin\
obj\
```

### build docker image
```
docker build -t creasant-test-webapi .
```

### run docker image as a container
```
docker run -d -p 8080:80 --name creasant-test-webapi creasant-test-webapi
```
