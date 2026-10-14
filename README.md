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
- **Checkboxes com Persistência**: Gravação instantânea do estado de conclusão no `localStorage` do navegador (`task_ID = true/false`).
- **Reordenação Inteligente**: Missões pendentes aparecem no topo; missões concluídas ganham texto riscado e vão automaticamente para o fim da lista.
- **Detalhes Expansíveis**: Ao clicar no cartão da missão, os objetivos são exibidos com etiquetas coloridas (`shoot`, `visit`, `giveItem`, `plantItem`, `extract`, `build`, etc.) e descrição em Português.
- **Redirecionamento Missão -> Mapa**: Botão **"Ver no Mapa"** que transita para a aba de Mapas, seleciona o mapa exato da missão, foca a câmera e posiciona um marcador tático pulsante.

### 🗺️ 4. Sistema Híbrido de Mapas (Leaflet + Embed)
- **Modo Mapa Tático (Leaflet.js)**:
  - Motor cartográfico baseado em coordenadas planas estáticas (`L.CRS.Simple`).
  - **Filtro de Alto Contraste / Modo Claro**: Inverte as linhas escuras dos SVGs do Tarkov.dev (`filter: invert(0.92) hue-rotate(180deg) contrast(1.25)`), tornando todas as plantas nítidas e legíveis sobre o tema escuro.
  - **Barra de Filtros por Camadas (`L.layerGroup`)**:
    - `[x] Missões Ativas` (Laranja `#ff9900`)
    - `[x] Extrações PMC` (Verde `#99cc00`)
    - `[x] Extrações Scav` (Azul `#3399ff`)
    - `[x] Chefes (Bosses)` (Vermelho `#ff3333`)
    - `[x] Locais Importantes / Hotspots` (Dourado `#c2aa80`)
  - **Normalização de Coordenadas**: Converte posições tridimensionais `{ x, y, z }` retornadas pela API para a escala plana `[0, 1000]` do Leaflet.
- **Modo Mapa Completo (Embed MapGenie / Tarkov.dev)**:
  - Iframe responsivo de alta resolução integrado diretamente no app.
  - Alternador dinâmico de fonte entre **MapGenie (Embed Escuro)** e **Tarkov.dev Oficial**.
- **The Lab Corrigido**: Endpoint de vetor oficial atualizado (`assets.tarkov.dev/maps/svg/Labs.svg`).

---

## 🛡️ Arquitetura de Conectividade e Resiliência (3 Camadas)

1. **Camada 1 (GraphQL Oficial em Português)**: Consulta completa com `items`, `ammo`, `maps` (com `extracts` e `bosses`) e `tasks` (com `zones`).
2. **Camada 2 (JSON CDN Estático em Português)**: Endpoints traduzidos resilientes `items_pt`, `tasks_pt` e `maps_pt`.
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
