name: Gerar Pac-Man
 
on:
  schedule:
    - cron: "0 */12 * * *"   # roda a cada 12 horas
  workflow_dispatch:           # permite rodar manualmente
  push:
    branches: [main]           # roda ao fazer push na main
 
jobs:
  generate:
    runs-on: ubuntu-latest
    timeout-minutes: 10
 
    steps:
      - name: Gerar animação Pac-Man
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/pacman.svg?color_snake=yellow&color_dots[]=9be9a8,40c463,30a14e,216e39
            dist/pacman-dark.svg?palette=github-dark&color_snake=yellow
 
      - name: Fazer push dos arquivos gerados
        uses: crazy-max/ghaction-github-pages@v3.1.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
 
