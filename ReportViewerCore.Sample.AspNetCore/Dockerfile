FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["ReportViewerCore.Sample.AspNetCore/ReportViewerCore.Sample.AspNetCore.csproj", "ReportViewerCore.Sample.AspNetCore/"]
COPY ["Microsoft.ReportViewer.NETCore/Microsoft.ReportViewer.NETCore.csproj", "Microsoft.ReportViewer.NETCore/"]
COPY ["Microsoft.ReportViewer.Common/Microsoft.ReportViewer.Common.csproj", "Microsoft.ReportViewer.Common/"]
COPY ["Microsoft.ReportViewer.DataVisualization/Microsoft.ReportViewer.DataVisualization.csproj", "Microsoft.ReportViewer.DataVisualization/"]
COPY ["Microsoft.ReportViewer.ProcessingObjectModel/Microsoft.ReportViewer.ProcessingObjectModel.csproj", "Microsoft.ReportViewer.ProcessingObjectModel/"]
RUN dotnet restore "ReportViewerCore.Sample.AspNetCore/ReportViewerCore.Sample.AspNetCore.csproj"
COPY . .
WORKDIR "/src/ReportViewerCore.Sample.AspNetCore"
RUN dotnet build "ReportViewerCore.Sample.AspNetCore.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "ReportViewerCore.Sample.AspNetCore.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ReportViewerCore.Sample.AspNetCore.dll"]
