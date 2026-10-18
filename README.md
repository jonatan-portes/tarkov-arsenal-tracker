# 🎯 Tarkov Arsenal & Quest Tracker

Uma Single Page Application (SPA) tática e responsiva para rastrear o arsenal de armas, munições, missões (tasks) e mapas interativos do jogo **Escape from Tarkov**, consumindo dados em tempo real da API pública do Tarkov com sistema de resiliência multi-camada (GraphQL, JSON CDN e Fallback Offline) e suporte completo ao idioma **Português (PT-BR)**.

---

## ⚡ Funcionalidades

### 🔫 1. Armas (Guns)
- Miniatura de cada item carregada via `iconLink` direto dos servidores de assets do Tarkov.
- Nomes e estatísticas traduzidos para Português.
- Filtragem automática de itens sem estatísticas de recuo vertical.
- Ordenação por **menor recuo vertical** (crescente).
- **Sistema dinâmico de Tiers**:
  - **Tier S** (Laranja Lendário `#ff9900`): Recuo vertical $\le 55$
  - **Tier A** (Verde `#99cc00`): Recuo vertical $\le 80$
  - **Tier B** (Azul `#3399ff`): Recuo vertical $\le 120$
  - **Tier D** (Vermelho `#ff3333`): Recuo vertical $> 120$
- Exibição de estatísticas balísticas: Recuo Vertical, Ergonomia, Cadência de Tiro e Calibre.

### 🎯 2. Munições (Ammo)
- Miniatura de cada projétil/cartucho puxada via `item.iconLink`.
- Nomes em Português e calibres formatados.
- Filtragem automática de munições sem poder de penetração.
- Ordenação por **maior poder de penetração** (decrescente).
- **Sistema dinâmico de Tiers**:
  - **Tier S** (Laranja Lendário `#ff9900`): Penetração $\ge 50$
  - **Tier A** (Verde `#99cc00`): Penetração $\ge 40$
  - **Tier B** (Azul `#3399ff`): Penetração $\ge 30$
  - **Tier D** (Vermelho `#ff3333`): Penetração $< 30$
- Exibição de atributos: Penetração, Dano Base, Dano em Armadura e Chance de Fragmentação.

### 📜 3. Missões (Tasks & Quests Tracker)
- **Checkboxes com Persistência**: Gravação instantânea do estado de conclusão no `localStorage` do navegador (`task_done_ID = true/false`).
- **Sistema de Missões Ativas (📌 Fixar / Ativa)**: Botão no cabeçalho de cada missão para destacar tarefas em andamento (`task_active_ID = true/false`), sincronizado com filtros rápidos (`Todas`, `📌 Ativas`, `Pendentes`, `Concluídas`).
- **Reordenação Inteligente**: Missões pendentes e ativas aparecem no topo; missões concluídas ganham texto riscado e vão automaticamente para o fim da lista.
- **Detalhes Expansíveis**: Ao clicar no cartão da missão, os objetivos são exibidos com etiquetas coloridas (`shoot`, `visit`, `giveItem`, `plantItem`, `extract`, `build`, etc.) e descrição em Português.
- **Redirecionamento Missão -> Mapa**: Botão **"Ver no Mapa"** que transita para a aba de Mapas, seleciona o mapa exato da missão, foca a câmera e posiciona um marcador tático com anel pulsante laranja.

### 🗺️ 4. Mapas Interativos (Leaflet + calibração oficial tarkov.dev)
- **10 mapas**: Customs, Woods, Factory, Shoreline, Interchange, Reserve, Lighthouse, Streets of Tarkov, Ground Zero e The Lab.
- **Pan/zoom fluido** com Leaflet 1.9.4 (`L.CRS.Simple`), usando o `maps.json` oficial do tarkov.dev (`bounds`, `transform`, `coordinateRotation`, `minZoom/maxZoom`): as posições `{x, y, z}` do jogo são projetadas com a mesma fórmula do tarkov.dev, então os ícones caem no local exato.
- **Seletor de andares** (Térreo, 2º/3º/4º/5º Andar, Subsolo, Túneis, Bunkers, Garagem...) gerado a partir dos `layers` de cada mapa; ícones de outros andares ficam esmaecidos.
- **Estilo Abstrato (SVG) / Satélite (tiles)** quando o mapa tem os dois, e botão de **Alto contraste**.
- **Camadas ligáveis**: Missões Ativas (pinos + zonas do objetivo), Extrações PMC (círculo verde) / Scav (quadrado azul) / cooperativas, Chefes (com chance de aparecer), Spawns PMC e Chaves & Locais (trancas e transições).
- **"Ver no Mapa"** nos objetivos da aba Missões: muda para a aba Mapas, abre o mapa e andar corretos, dá zoom na zona do objetivo e abre um balão com a missão. Objetivos sem local fixo (ex.: "elimine X em qualquer lugar") avisam isso em vez de criar um pino falso.
- **Painel "Missões Ativas neste mapa"** com botão "Localizar" por objetivo.
- **Contingência**: sem Leaflet/calibração, o mapa oficial do tarkov.dev pode ser carregado em iframe.
- Fontes: `raw.githubusercontent.com/the-hideout/tarkov-dev/.../maps.json` (calibração), `json.tarkov.dev/regular/maps(_pt)` e `/tasks` (extrações, chefes, spawns, trancas e zonas de objetivos) — carregados só quando a aba Mapas é aberta.

---

## 🛡️ Arquitetura de Conectividade e Resiliência (3 Camadas)

1. **Camada 1 (GraphQL Oficial em Português)**: Consulta com `items`, `ammo` e `tasks` (as posições de mapa vêm sempre do JSON CDN, na aba Mapas).
2. **Camada 2 (JSON CDN Estático em Português)**: Endpoints traduzidos resilientes `items_pt` e `tasks_pt` (mais `maps`/`maps_pt` na aba Mapas).
3. **Camada 3 (Fallback Offline)**: Dataset embutido 100% em Português com suporte completo a armas, munições, missões clássicas, extrações e hotspots.

---

## 🎨 Paleta de Cores e Design Tático

| Elemento | Cor | Hexadecimal |
|---|---|---|
| Fundo Principal | Preto Tático Profundo | `#0a0a0a` |
| Fundo dos Cartões | Cinza Militar Escuro | `#151515` |
| Borda e Detalhes | Dourado / Oliva | `#9a8866` / `#c2aa80` |
| Texto Principal | Branco Fosco | `#e0e0e0` |
| Texto Secundário | Cinza Neutro | `#888888` |
| Missões Ativas | Laranja Lendário | `#ff9900` |
| Extrações PMC | Verde Operacional | `#99cc00` |
| Extrações Scav | Azul Tático | `#3399ff` |
| Chefes (Bosses) | Vermelho Alerta | `#ff3333` |

---

## 🚀 Como Executar

Abra diretamente no navegador:

```bash
# No Windows PowerShell:
Start-Process .\index.html
```

---

## 👤 Autor

Desenvolvido por **[Jonatan Portes](https://github.com/jonatan-portes)**.
