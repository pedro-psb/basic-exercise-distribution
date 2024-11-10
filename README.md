# Banco de Exercícos ICEX/UFMG

Especificação e implementação de um [Repositório (software)](https://pt.wikipedia.org/wiki/Reposit%C3%B3rio_(software))
de exercícios **com resolução** chamado Bed (Basic Exercise Distribution).

Esse repositório git contém:
* Um Manifesto das motivações desse projeto.
* Especificação do Bed (Basic Exercise Distribution), um novo tipo de repositório para distribuir exercícios (similar a um [repositório rpm](#)).
* Uma ferramenta auxiliar para manejar repositórios bed (checar consistencia, gerar metadados, etc)
* Especificação do conteúdo básico do repositório, "bed-file", que é um markdown (GitHub-flavored) com uma estrutura pre-determinada.
* Uma ferramenta para fazer lint, format e outras operações em um bed-file.
* Um CI para automatizar checagens de PRs e criação de releases.

## Objetivos

### Primário

Possibilitar um processo consistente de contribuição, revisão e distribuição de exercícios da área de ciências exatas.

Especificamente, para suprir casos de falta ou atraso na disponibilização de materiais de prática a alunos de graduação que cursam disciplinas no ICEX/UFMG.

### Secundário

Popularizar o `LaTex` e processos de contribuição open-source entre alunos de graduação da área de ciências exatas.

## Uso

TODO: Apresentação geral do projeto, do bed-file, do bed-repo e de workflows para:
* contribuir
* revisar
* compilar listas (curadorias)

## Fluxo geral

1) Contribuidor:
    * Faz um fork e cria uma feature-branch
    * Cria arquivo de exercico no `workdir/` usando a [variação-markdown do GitHub](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).
    * Cria commit localmente, faz push para fork e [abre PR no GitHub](https://docs.github.com/pt/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request).
2) Revisor:
    * Revisa PR, pede/sugere modificações e aprova
3) Auto-release
    * Em intervalos regulares, CI-bot cria PR de release
    * O PR de release é resultado de:
        - `build`, que movimenta `workdir/* -> bed_repo/data/*`.
        - `publish`, que cria o metadado do repositório `bed_repo/BedMetadata.json`
        - `bump`, que atualiza a versão (CalVer)
    * Sua aprovação gera:
        - Nova tag no github e publicação dos metadados gerados `BedMetadata.json`
        - Publicação no PyPi dos pacotes `bed_lib` e `bed_cli`.

## Estutura do projeto

```bash
# CI
.github/workflows

# Makefile com comandos para `build`, `publish`, `format` e similares.
Makefile

# Pasta raíz para utilitarios no gerenciamento de arquivo e do repositório
tooling/
    bed_cli/
    bed_lib/

# Espaço de trabalho para criar exercicio
workdir/
    INSTRUÇÔES.md

# Repositório Bed
# * Arquivos usam UUIDv7. Deve ser criado com utilitario de `manager`
# * BedMetadata.json:
#     Metadata do repositório. Lista todos os exercícios disponíveis.
#     É criado atravéz de um comando de publicar ("publish").
bed_repo/
    BedMetadata.json
    data/
      01/931254-087f-77be-afd9-744ea4be3260.md
      01/931254-087f-7416-b75d-da1933683e4f.md
      ...
      02/931254-941f-783e-984b-44db98f98a7f.md

# Repositório Bed efêmero
# * Objetivo é possiblitar pessoas a praticarem o processo de contribuir/revisar
# * É limpado de tempos em tempos
hello_world/
    BedMetadata.json
    data/
      01/931254-087f-77be-afd9-744ea4be3260.md
      01/931254-087f-7416-b75d-da1933683e4f.md
      ...
      02/931254-941f-783e-984b-44db98f98a7f.md
```

