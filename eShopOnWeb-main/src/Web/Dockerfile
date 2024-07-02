# RUN ALL CONTAINERS FROM ROOT (folder with .sln file):
# docker-compose build
# docker-compose up
#
# RUN JUST THIS CONTAINER FROM ROOT (folder with .sln file):
# docker build --pull -t web -f src/Web/Dockerfile .
#
# RUN COMMAND
#  docker run --name eshopweb --rm -it -p 5106:5106 web


# FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
# WORKDIR /app

# COPY *.sln .
# COPY . .
# WORKDIR /app/src/Web/
# RUN dotnet restore 

# RUN dotnet publish -c Release -o out

# FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
# WORKDIR /app
# EXPOSE 5106
# EXPOSE 5200
# EXPOSE 5300

# COPY --from=build /app/src/Web/out ./

# ENTRYPOINT ["dotnet", "Web.dll"]


# Stage 1: Build the application
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app

# Copy the solution file and restore dependencies
COPY *.sln .

# Copy the rest of the application code
COPY . .

# Build and publish the project
WORKDIR /app/src/Web
RUN dotnet restore

RUN dotnet publish -c Release -o out

# Stage 2: Create the final image
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
WORKDIR /app
COPY --from=build /app/src/Web/out ./

# Expose port 80
EXPOSE 80

# Specify the DLL to run
ENTRYPOINT ["dotnet", "Web.dll"]