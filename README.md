# 🎯 Tarkov Arsenal & Quest Tracker

Uma Single Page Application (SPA) tática e responsiva para rastrear o arsenal de armas, munições e missões (tasks) do jogo **Escape from Tarkov**, consumindo dados em tempo real da API pública do Tarkov com sistema de resiliência multi-camada (GraphQL, JSON CDN e Fallback Offline).

---

## ⚡ Funcionalidades

### 🔫 1. Armas (Guns)
- Miniatura de cada item carregada via `iconLink` direto dos servidores de assets do Tarkov.
- Filtragem automática de itens sem estatísticas de recuo vertical.
- Ordenação por **menor recuo vertical** (crescente).
- **Sistema dinâmico de Tiers**:
  - **Tier S** (Laranja Lendário `#ff9900`): Recuo vertical $\le 55$
  - **Tier A** (Verde `#99cc00`): Recuo vertical $\le 80$
  - **Tier B** (Azul `#3399ff`): Recuo vertical $\le 120$
  - **Tier D** (Vermelho `#ff3333`): Recuo vertical $> 120$
- Exibição de estatísticas vitais: Recuo Vertical, Ergonomia, Cadência de Tiro e Calibre.

### 🎯 2. Munições (Ammo)
- Miniatura de cada projétil/cartucho puxada via `item.iconLink`.
- Filtragem automática de munições sem poder de penetração.
- Ordenação por **maior poder de penetração** (decrescente).
- **Sistema dinâmico de Tiers**:
  - **Tier S** (Laranja Lendário `#ff9900`): Penetração $\ge 50$
  - **Tier A** (Verde `#99cc00`): Penetração $\ge 40$
  - **Tier B** (Azul `#3399ff`): Penetração $\ge 30$
  - **Tier D** (Vermelho `#ff3333`): Penetração $< 30$
- Exibição de atributos balísticos: Penetração, Dano Base, Dano em Armadura e Chance de Fragmentação.

### 📜 3. Missões (Tasks & Quests Tracker)
- **Checkboxes com Persistência**: Ao marcar ou desmarcar uma missão, o estado é gravado automaticamente no `localStorage` do navegador (`task_ID = true/false`).
- **Reordenação Inteligente**: Missões pendentes aparecem no topo; missões já concluídas recebem estilo visual esmaecido com texto riscado e são posicionadas automaticamente ao final da lista.
- **Detalhes Expansíveis (Accordion)**: Ao clicar no cartão da missão, os objetivos são desdobrados mostrando:
  - Tipo de objetivo (`shoot`, `visit`, `giveItem`, `plantItem`, `extract`, etc.) com tag colorida e ícone tático.
  - Descrição da tarefa operacional.
  - Localização geográfica: Nome do mapa (Customs, Woods, Factory, Shoreline, Ground Zero, etc.) ou indicação `"Qualquer lugar / Global"`.
- **Barra de Progresso**: Monitoramento em tempo real do número de missões concluídas, pendentes e porcentagem total de progresso da conta.

---

## 🛡️ Arquitetura de Conectividade e Resiliência (3 Camadas)

1. **Camada 1 (GraphQL Oficial)**: Conecta-se a `https://api.tarkov.dev/graphql` executando a query especificada (`items`, `ammo`, `tasks`).
2. **Camada 2 (JSON CDN Estático)**: Caso o servidor GraphQL reporte instabilidade ou código 422/503, a aplicação se conecta de forma transparente ao CDN de dados estáticos (`json.tarkov.dev/regular/items`, `tasks`, `maps_en`).
3. **Camada 3 (Fallback Offline)**: Em caso de ausência total de internet, dados de amostra embutidos com armas, munições e missões clássicas garantem que a interface nunca quebre.

---

## 🎨 Paleta de Cores e Design Tático

| Elemento | Cor | Hexadecimal |
|---|---|---|
| Fundo Principal | Preto Tático Profundo | `#0a0a0a` |
| Fundo dos Cartões | Cinza Militar Escuro | `#151515` |
| Borda e Detalhes | Dourado / Oliva | `#9a8866` / `#c2aa80` |
| Texto Principal | Branco Fosco | `#e0e0e0` |
| Texto Secundário | Cinza Neutro | `#888888` |
| Tier S | Laranja Lendário | `#ff9900` |
| Tier A | Verde Operacional | `#99cc00` |
| Tier B | Azul Tático | `#3399ff` |
| Tier D | Vermelho Crítico | `#ff3333` |

---

## 🚀 Como Executar

Por ser uma SPA construída com HTML5, CSS3 moderno e Vanilla JavaScript (sem dependências externas ou bundlers necessários), basta abrir o arquivo diretamente no navegador:

```bash
# No Windows PowerShell:
Start-Process .\index.html
```

Ou através de qualquer servidor local simples:

```bash
# Usando npx:
npx serve .
# Ou usando Python:
python -m http.server 8080
```

---

## 👤 Autor

Desenvolvido por **[Jonatan Portes](https://github.com/jonatan-portes)**.
