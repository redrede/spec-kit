> 🇧🇷 Esta documentação está em Português do Brasil. [English version](../README.md)

# Documentação

Esta pasta contém os arquivos fonte da documentação para o Spec Kit, construídos usando [DocFX](https://dotnet.github.io/docfx/).

## Construindo Localmente

Para construir a documentação localmente:

1. Instale o DocFX:

   ```bash
   dotnet tool install -g docfx
   ```

2. Construa a documentação:

   ```bash
   cd docs
   docfx docfx.json --serve
   ```

3. Abra seu navegador em `http://localhost:8080` para visualizar a documentação.

## Estrutura

- `docfx.json` - Arquivo de configuração do DocFX
- `index.md` - Página inicial da documentação principal
- `toc.yml` - Configuração de índice
- `installation.md` - Guia de instalação
- `quickstart.md` - Guia de início rápido
- `_site/` - Saída da documentação gerada (ignorada pelo git)

## Implantação

A documentação é automaticamente construída e implantada no GitHub Pages quando alterações são enviadas para a branch `main`. O workflow é definido em `.github/workflows/docs.yml`.
