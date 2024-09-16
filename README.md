# HeatWise

HeatWise é uma solução de análise de produtividade focada em dados de interações de usuários em sistemas e softwares. Ele permite que as empresas transformem esses dados em insights acionáveis, otimizando a performance e eficiência de suas equipes.

## Índice

- [Descrição](#descrição)
- [Funcionalidades](#funcionalidades)
- [Como Usar](#como-usar)
- [Deploy no Azure](#deploy-no-azure)


## Descrição

HeatWise coleta, processa e analisa dados de produtividade de interações em softwares. A plataforma foi desenvolvida para atender a necessidade de transformar dados brutos em informações valiosas que auxiliam no aumento da eficiência dos negócios.

## Funcionalidades

- Coleta de dados de interações de usuários em diferentes sistemas
- Análise de dados com métricas de produtividade
- Geração de relatórios e gráficos detalhados
- Integração com diferentes plataformas de software
- Interface intuitiva para visualização de dados em tempo real

## Como Usar 
Após a instalação, a aplicação estará disponível localmente no endereço da porta configurada no Azure.

## Deploy no Azure

HeatWise será deployado no Microsoft Azure para proporcionar escalabilidade e disponibilidade. Uma vez que o deploy estiver configurado, um pipeline de CI/CD será criado utilizando um arquivo .yml para automatizar o processo de build e deploy.

### Exemplo de integração com GitHub Actions:

```yml
on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  build:
    runs-on: windows-latest

    steps:
      - uses: actions/checkout@v4

      - name: Set up .NET Core
        uses: actions/setup-dotnet@v4
        with:
          dotnet-version: '8.x'

      - name: Build with dotnet
        run: dotnet build "HeatWise-Sprint 2.Net.sln" --configuration Release

      - name: dotnet publish
        run: dotnet publish "HeatWise-Sprint 2.Net.sln" -c Release -o "C:\Program Files\dotnet\myapp"

      - name: Upload artifact for deployment job
        uses: actions/upload-artifact@v4
        with:
          name: .net-app
          path: ${{env.DOTNET_ROOT}}/myapp

  deploy:
    runs-on: windows-latest
    needs: build
    environment:
      name: 'Production'
      url: ${{ steps.deploy-to-webapp.outputs.webapp-url }}
    permissions:
      id-token: write # This is required for requesting the JWT

    steps:
      - name: Download artifact from build job
        uses: actions/download-artifact@v4
        with:
          name: .net-app```
