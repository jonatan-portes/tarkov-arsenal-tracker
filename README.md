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

### 🗺️ 4. Sistema Híbrido Split-Screen de Mapas & Painel de Raid
- **Coluna Esquerda: Painel de Raid Tático (320px)**:
  - **Seletor de Mapas Integrado**: Alternância instantânea de mapas com indicador de missões ativas.
  - **📌 Missões Ativas neste Mapa**: Monitoramento em tempo real com caixas de seleção interativas para concluir objetivos sem sair da raid (`task_done_ID` gravado ao vivo no `localStorage`).
  - **📍 Missões Gerais do Mapa (Sem local fixo)**: Identificação clara de missões com objetivos globais (ex: eliminações ou coletas em qualquer setor) sem poluir o mapa com pinos artificiais no centro `[500, 500]`.
  - **Outras Missões Pendentes**: Accordion expansível para consulta rápida de tarefas secundárias no mapa selecionado.
- **Coluna Direita: Visualizador de Mapas (1fr)**:
  - **Modo Principal: Tarkov.dev Oficial (Embed)**: Visualização de alta fidelidade com todos os andares, extrações, spawns e loot oficial do Tarkov.dev (com alternador para MapGenie).
  - **Modo Tático: Leaflet Calibrado Matematicamente**:
    - **Calibração Oficial Tarkov.dev**: Carregamento dinâmico do `maps.json` oficial com matriz de rotação (`coordinateRotation`: 0, 90, 180, 270) e projeção de limites reais (`bounds`), garantindo alinhamento pixel-perfect sobre os SVGs.
    - **Sem Pinos Falsos**: Remoção total do fallback arbitrário `[500, 500]` e de fórmulas artificiais para chefes (`[480 + idx*40, 500 + idx*30]`). Chefes e objetivos só geram marcadores em posições pontuais reais ou hotspots verificados.
    - **Seletor de Andares Multi-Nível (Floors)**: Filtro dinâmico para mapas verticais complexos (`Todos`, `B1 / Subsolo`, `1º Andar`, `2º Andar / Telhado`).
    - **Filtro de Alto Contraste / Modo Claro**: Inverte as linhas escuras dos SVGs do Tarkov.dev (`filter: invert(0.92) hue-rotate(180deg) contrast(1.25)`).
    - **Barra de Filtros por Camadas (`L.layerGroup`)**: Missões (Ativas/Pendentes), Extrações PMC, Extrações Scav, Chefes (Bosses) e Locais Importantes (Hotspots).
- **GraphQL Otimizado (`TaskObjectiveShoot`)**: Consulta estendida para trazer coordenadas reais de zonas de eliminação.

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
