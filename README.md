# Central Tática

Site de consulta para Escape from Tarkov. Reúne armas, munições, missões, preços de itens e mapas interativos em uma página só, com dados da API pública do tarkov.dev. Funciona em português e em inglês, e os nomes de missões, itens e locais vêm do jogo, não de tradução automática.

É um projeto de fã, sem vínculo com a Battlestate Games. A logo no cabeçalho pertence a eles.

## O que dá para fazer

### Armas

Lista as armas ordenadas pelo menor recuo vertical, junto com ergonomia, cadência de tiro e calibre. Cada arma recebe um tier pelo recuo: S até 55, A até 80, B até 120 e D acima disso. Armas sem dado de recuo ficam de fora.

### Munições

Mostra penetração, dano, dano em armadura e chance de fragmentação, da maior penetração para a menor. O tier vem da penetração: S a partir de 50, A a partir de 40, B a partir de 30 e D abaixo disso. Abaixo da busca há botões de calibre (5.45x39, 7.62x51 e assim por diante) para ver só um tipo de munição.

### Missões

- Marque missões como concluídas. O estado fica salvo no navegador, então não precisa de conta.
- Fixe as missões que está fazendo e filtre por todas, ativas, pendentes ou concluídas. Pendentes e ativas aparecem primeiro, e as concluídas vão para o fim.
- Abra uma missão para ver os objetivos e a lista de itens necessários: chaves, itens que você precisa entregar, plantar ou usar. Itens que a própria missão manda você achar na raide não entram na lista.
- O botão "Ver no mapa" abre o mapa certo, no andar certo, com zoom no local do objetivo.

### Itens

Catálogo com preço no Flea Market, melhor venda e menor compra nos traders, por categoria. Dá para ordenar por preço e buscar pelo nome ou pela sigla do item.

### Mapas

- Dez mapas: Customs, Woods, Factory, Shoreline, Interchange, Reserve, Lighthouse, Streets of Tarkov, Ground Zero e The Lab.
- Zoom e arrasto com as coordenadas oficiais do tarkov.dev, então os ícones caem no lugar certo.
- Seletor de andares nos mapas que têm mais de um nível.
- Estilo abstrato ou satélite, nos mapas que têm os dois.
- Camadas que você liga e desliga: missões ativas, extrações PMC e Scav, chefes com a chance de aparecer, spawns PMC, trancas e transições entre mapas.
- O nome de cada extração aparece sobre o ícone, sem precisar clicar.
- Um painel lista as missões ativas do mapa aberto, com botão para localizar cada objetivo.
- Se o mapa interativo não carregar, dá para abrir o mapa oficial do tarkov.dev dentro da página.

### Idioma

As bandeiras no canto do cabeçalho trocam entre português e inglês. A interface foi traduzida à mão, e os dados do jogo são pedidos à API já no idioma escolhido.

## Tecnologias

- HTML, CSS e JavaScript puro, tudo em um único `index.html`, sem build e sem dependências para instalar.
- Leaflet 1.9.4 para os mapas.
- API do tarkov.dev (GraphQL e JSON estático) para armas, munições, missões, itens e posições.
- `localStorage` para guardar missões concluídas e ativas.

## De onde vêm os dados

A página tenta três fontes, nesta ordem:

1. A API GraphQL do tarkov.dev.
2. Os arquivos JSON estáticos do tarkov.dev (`items`, `tasks`, `maps`), no idioma escolhido.
3. Um conjunto pequeno de dados embutido na página, usado só quando as duas primeiras falham. Ele é uma amostra reduzida e não acompanha a troca de idioma.

As posições do mapa (extrações, chefes, spawns, trancas e zonas de objetivo) vêm sempre do JSON e só são baixadas quando você abre a aba Mapas.

## Como executar

Abra o `index.html` no navegador. No Windows PowerShell:

```bash
Start-Process .\index.html
```

Precisa de internet para buscar os dados e carregar o Leaflet.

## Autor

Feito por [Jonatan Portes](https://github.com/jonatan-portes).
