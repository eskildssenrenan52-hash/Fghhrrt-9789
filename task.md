# CAMPEÕES MECHA — progresso

## Merge 2026-10-10 — repo Yek2kue-0408 + mundo G5
- [x] Repo GitHub mesclado via git (base comum 479b1c9). Backup do estado anterior no branch `backup-g5-pre-merge`. Único conflito: task.md (texto).
- [x] Tipo `Screen` movido para `game/screen-ids.ts` (typecheck do mobile quebrava via cadeia API→web/game→screens/menu).
- [x] `launch-assets.ts`: removidos 17 arquivos inexistentes (/map/tiles, /map/props antigos) e incluídos 62 de /world.
- [x] Botão PULAR ABERTURA movido para o topo (o selo Runable cobria o botão no rodapé).
- [x] db:push aplicado (tabelas de auth/online). typecheck 3/3 e build OK.
- [x] `assets/e2e_world_g5.py` PASS: entrada nova → JOGAR → hub → COLISEU → TORNEIOS [T] (54 liberados no nível 20) → briefing → batalha real (3 golpes); teclado, joystick proporcional (−100 e soltura 0), atlas, diário; zero erro de console/HTTP.
- [ ] Não testado nesta rodada: PvP online/login, tutorial completo, corrida/cerco, lanternas, migração mc_world_g4.
- Tela de vitória MVP: cancelada pelo usuário — não implementar.
- APK: gerado pelo usuário via Publicar na prévia mobile (conta Expo). Não buildar no sandbox.


## Novo escopo aprovado — PvP online real e conclusão das pendências
- Aprovado: fila pública + salas privadas por código; equipe de até quatro robôs; turnos de 60 segundos; reconexão; ações/tempo/resultado validados no servidor, nunca adversários IA disfarçados de players.
- Competitivo normalizado: todos os 256 robôs disponíveis; nível padronizado; sem equipamentos/pets/cards ou bônus oriundos de saves offline editáveis. Saves offline preservados e separados do competitivo.
- Remover AUTO em TODAS as batalhas, inclusive no motor/API; IA continua exclusivamente como adversário dos modos offline.
- Login só para PvP: Google gerenciado + e-mail/senha, tela única com abas, retorno ao lobby depois de autenticar. Jogar offline não exige conta.
- Implementação prevista em packages/web: auth/middleware e tabelas Drizzle de contas/salas/fila/partidas/ações/resultados; procedimentos oRPC para lobby/equipe/salas/fila/estado/ações/desistência; consultas tipadas e novas telas de acesso/lobby/batalha online. Servidor reutiliza regras compartilhadas sem executar decisões de IA no PvP. Atomicidade e idempotência obrigatórias.
- Pendências offline mantidas: auditoria completa de status/passivas/talentos, balanceamento, objetivos obrigatórios do tutorial, sequências exclusivas IA restantes. Sem locução, conforme confirmação; abertura/tutorial continuam sem voz. Sem E2E/Playwright/gameplay em navegador.
- BLOQUEIO constatado antes da implementação online: não existe .env no projeto e não há DATABASE_URL/DATABASE_AUTH_TOKEN/APPLICATION_ID/issuer no ambiente. Backend atual tem só ping e nenhuma autenticação. Credenciais reais de banco necessárias, sem placeholders. Não declarar PvP conectado ou concluído.

## Checkpoint atual — terreno, clima e absorção elemental
- [x] Climas filtram golpes de área/contundentes e elementos corretamente; ar rarefeito afeta todo dano, eclipse penaliza LUZ/beneficia SOMBRA, branco absoluto reduz crítico. Arenas derivadas herdam as regras.
- [x] Zonas de cura/MP/perigo aplicam efeitos por round; terrainMaster duplica benefícios reais e hazardWalker evita apenas dano de zona/converte em 6 MP. Vácuo bloqueia cura de terreno, não reparos; zonas void ampliam dano recebido sem bloquear cura. Sem ataques automáticos novos da reserva.
- [x] Buff de força da zona deixa de ser contado novamente no poder do golpe. marked/maldição/vulnerabilidade ampliam dano em vez de reduzi-lo. Clima/status drain retiram porcentagem do MP máximo; healIn modifica cura recebida, não produz cura espontânea.
- [x] Afinidade zero cura o alvo e não produz dano mínimo/status hostil/carga por dano recebido. Projeção de dano usa strike em cópias isoladas, incluindo multihit each, barreira/escudo/parry/passivas/clima; projeções probabilísticas são sinalizadas e não garantem todos os resultados de RNG.
- [x] IDs de golpe inválidos não viram ataque básico nem consomem turno pronto. Troca forçada só aceita fase forced_swap e índice inteiro vivo.
- [x] Tutorial orienta pelos painéis atuais de ataques/defesas/ultimate/SUPRIMENTOS, sem esferas obsoletas. Objetivos pedagógicos obrigatórios ainda NÃO implementados.
- [x] Dez suites passaram, TypeScript/build e validação de arte/HTTP passaram. Simulações das cinco missões: vitória em 3/4/8/6/4 rounds. Saves legado/vazio preservados. Sem E2E, Playwright ou gameplay em navegador; avisos conhecidos de Bun/Node/chunk permanecem.
- [ ] Ainda pendentes: auditoria exaustiva de status/talentos/passivas (incluindo thornMine e outras condições de terreno), balanceamento completo, objetivos pedagógicos obrigatórios, demais sequências exclusivas e plano de locução aprovado. Não declarar refinamento completo.

## Checkpoint atual — passivas, segurança e interface anime mecha
- [x] Holograma/orbs removidos da batalha; quatro ataques e duas defesas sempre renderizados com nomes/custos, informação separada, ultimate em painel próprio, retratos/HUD maiores, deck inferior responsivo e molduras pixel existentes. Estrutura verificada por renderização React no servidor; não verificada visualmente em navegador, conforme restrição.
- [x] Hooks de início/fim de turno, erro, dano recebido, crítico e pós-ataque conectados; reações diretas não acionam recursivamente outras reações. Reserva não produz dano de passiva de abertura de round. Bônus escopados limitados, reiniciados por round.
- [x] Núcleos fênix/imortal e segundo fôlego com consumo único; reviver antes de conceder abate. Primeiro abate do portador cura uma vez; onFaint duplicado removido. Predador usa HP do alvo, guarda completa anula golpe e prisma reflete em vez de aumentar dano recebido.
- [x] onAllyFaint/cura coletiva, onMpSpend, onPurge e onStatusApply conectados; statusWeaver/pyromaniac ajustam duração/potência, purgeAura remove um negativo e battleRhythm responde à alternância ataque/defesa.
- [x] Mineração rejeita NaN/Infinity/minérios desconhecidos/profundidades inválidas/tempo regressivo; quantidades discretas. Refinaria/venda têm consumo único. Sockets inteiros e proprietário obrigatório; laboratório valida peças/núcleo/hue antes de gastar e desmontagem conserva equipe válida.
- [x] Callbacks finais de ranked/torneios protegidos contra repetição local; ranked conta sobreviventes só da equipe real, não entradas auxiliares do snapshot.
- [x] Nove suites passaram (seis anteriores + passive-lifecycle, economy-safety e battle-command-layout). Tutorial simulado 3/4/8/6/4 rounds, legado e vazio preservados. TypeScript/build passaram; avisos conhecidos de chunk >500kB e Node permanecem.
- [x] HTTP 200 da prévia; 26 assets de entrada/especiais e 15 referências de imagem da interface com MIME/hash corretos. Sem E2E, Playwright ou gameplay em navegador.
- [ ] Ainda pendentes: paridade exaustiva de todas as descrições/passivas/terreno/talentos, equilíbrio, objetivos pedagógicos obrigatórios do tutorial, demais sequências exclusivas dos robôs atuais e aprovação de plano de locução. Este checkpoint NÃO conclui todos os itens restantes.

## Escopo atual e checkpoint verificado
- Expansão de novos robôs CANCELADA pelo usuário. Manter 256 recrutáveis + oito NPCs, sem produzir/cadastrar mc_257…mc_500. Manifestos antigos ficam como histórico, não trabalho ativo.
- Nenhum E2E, Playwright ou gameplay no navegador. Não executar scripts de reconstrução antigos.
- [x] Remulu Games: logo IA sobre preto com fade, pular, timeout/fallback e sem voz.
- [x] Novo carregamento: 50 frames, três fundos, progresso real de imagens, falhas explícitas e tentativa novamente; peças modulares também reportam falhas e permitem nova tentativa.
- [x] Ultimate dentro da arena: ator/alvo reais, HUD preservado; dez pares exclusivos recortados, demais usam sprite e VFX existentes. Timeout de arte evita bloqueio. Não declarar todas as sequências exclusivas concluídas.
- [x] Compras e treino com saldo atual; equipe única de até quatro, equipe não vazia quando há robôs; briefing de todos os sistemas e rota por nível.
- [x] Golpes carregados pagam MP/recurso uma vez, respeitam multihit, não duplicam carga e limpam preparação na queda. UI distingue preparo/liberação e bloqueia golpes silenciados/impagáveis.
- [x] Cura coletiva de teamPlayer e disparo onUltReady de ultimateBattery; descrições atualizadas. COBRIR explícito protege aliado, sem ataques automáticos da reserva.
- [x] Dimensões: draft exige exatamente quatro únicos; campanha válida; carry HP/MP finito em 0–1 e núcleo em 0–100; fases e recompensas idempotentes testadas.
- [x] Usina/capacitor, expedição/pesquisa, cargas da academia/talentos, ressonância do museu, resgates encadeados/marcos dos contratos testados. Inputs inválidos da expedição e história não alteram save.
- [x] Equipamentos/skins exigem robô e skin possuídos; venda repetida não duplica ouro. Tutorial não paga antes de iniciar.
- [x] Seis suites estáticas passaram: combat-refinement, city-refinement, polish, overhaul, legacy, empty. Cinco missões simuladas vencidas em 3/3/7/6/4 rounds; saves legado e vazio preservados nos testes.
- [x] TypeScript e build passaram (204 módulos). Avisos: chunk >500 kB e depreciação Node; sem erro de build.
- [x] Validação pós-build: 26 imagens HTTP 200 com MIME de imagem e hash idêntico ao arquivo, 50 células de loader distintas e 20 strips transparentes com dez células distintas. Preview HTTP 200.
- [ ] Refinamento profundo restante: auditar todas as passivas/hooks/status/talentos, mineração/cards/pets/laboratório/ranked/torneios e equilíbrio completo. Briefing não substitui mecânica auditada.
- [ ] Sequências IA exclusivas de ultimate/VFX dos demais robôs atuais. Nenhuma nova identidade pendente.
- [ ] Locução do tutorial somente após aprovação de plano de voz/custo/duração. Abertura permanece sem voz.

## Overhaul MECHA CHAMPIONS — aprovado, em execução (2026-10-07)
- Decisões e curva de carga/desbloqueios registradas em design.md. Todos os lotes IA autorizados diretamente; não executar E2E/Playwright.
- [x] Motor: remover combo coletivo, ultimate individual sem MP, carga por ação/dano, IA e loadout 4+2 (regras testadas; auditoria total pendente).
- [x] Arte de interface: esfera, dois hologramas 25 frames, título e símbolos; inspeção e recorte.
- [x] Interface de batalha: comandos orbitais, informação separada, especial na arena e fallbacks.
- [x] Entrada, estado vazio válido, migração sem apagar saves e cinco batalhas de tutorial.
- [x] Desbloqueios por nível em prédios, atalhos e navegação; HUD de piloto.
- CANCELADO: 244 novos e respectivos lotes. Não retomar.
- [x] Verificação estática de regras, TypeScript/build, dimensões/contagem/HTTP. Entrega do checkpoint, não conclusão de toda a auditoria.

## Cidade imersiva — seis sistemas (2026-10-07)
- Escopo aprovado: seis experiências únicas + mecânicas aprofundadas; lotes IA completos sem prévia. Só web; sem E2E/Playwright.
- [x] Direção registrada em design.md; seis cenários animados, dez portais 16 frames e 24 relíquias gerados.
- [x] Inspeção/recorte e assets públicos: seis folhas 12 frames, dez folhas 16 frames e 24 relíquias repaginadas em 6×4. Dimensões e células distintas verificadas.
- [x] Migração implementada: protocolos/capacitor, rotas pesquisadas, simulações, ressonância, rastreamento/marcos, calibração/tier dimensional. Distribuição sobrecarregada ajustada sem apagar demais campos do save.
- [x] Seis telas próprias ligadas às ações persistentes; animações pausam fora da área visível e respeitam movimento reduzido.
- [x] TypeScript e Vite build passaram; preview e todos os 25 assets novos responderam HTTP 200. Nenhum E2E/Playwright nem teste de gameplay no navegador executado.

## Batalha imersiva (2026-10-07)
- [x] Oito materiais de chão gerados por IA, inspecionados e exportados WebP: madeira, aço, lava, gelo, cristal, floresta, carpete real e mosaico abissal. Mapeados às 41 arenas com fallback.
- [x] Mundo separado do HUD: plano panorâmico, robôs afastados e redimensionados, plataformas em perspectiva, piso contínuo e sombras de contato.
- [x] Câmera por eventos reais: antecipa ações anunciadas, aproxima autor/entrada/ultimate, acompanha impactos e retorna ao plano aberto no fim da fila. UIDs mostram o participante correto mesmo em ações da reserva.
- [x] Projéteis percorrem a distância real entre lados, avanços ajustados ao palco. Console centralizado e rolável, HUD/ordem sem zoom. Câmera fixa e movimento reduzido desativam câmera/tremores; movimento reduzido oculta VFX de tela.
- [x] TypeScript/build aprovados após integração. Sem alterações deliberadas nas regras de dano, recursos, fila ou recompensas; gameplay visual não verificado, conforme proibição de E2E/Playwright.

## Vitória cinematográfica (2026-10-07)
- [x] Três imagens geradas por IA e inspecionadas: duas folhas 5×5 com 25 frames distintos cada; um santuário animado 3×3 com 9 frames distintos.
- [x] Assets WebP em public/victory; reprodução de 50 frames a ~14 fps com luz contínua e dissolução de 850 ms. Pré-carregamento durante a batalha, timeout de segurança e botão de pular.
- [x] Vitória integrada ao encerramento real do BattleScreen, somente após esvaziar os eventos. Derrota preserva relatório anterior; continuação mantém gravação e snapshot existentes, sem duplicar recompensas.
- [x] Tela dividida: arte/MVP aliado à esquerda, resultado/estatísticas reais/abas de equipe, adversários e cronologia à direita; empilhamento em largura pequena.
- [x] Foco de diálogo, navegação por teclado, movimento reduzido e proteção de clique duplicado.
- [x] TypeScript e Vite build passaram; HTTP do preview respondeu 200. Sem execução de E2E/Playwright ou verificação visual de gameplay no navegador.

## Expansão da cidade e dimensões (2026-10-05)
- Plano aprovado: 6 construções físicas (Observatório, Academia, Contratos, Usina, Museu, Portal).
- 10 modos aprovados: Draft, Caravana, Fenda, Espelhos, Cerco, Caça, Reatores, Monoelemental, Apostas, Labirinto.
- Arte: lotes de vários assets por imagem; inimigos exclusivos com 16 frames/chassi, não recrutáveis; 10 arenas novas com 4 frames.
- Sem E2E/Playwright conforme instrução do usuário. Só TypeScript e build.
- [ ] Integrar VFX gl/ult/imp/st_burst/sup/wld e 14 cenários antigos.
- [ ] Recortar e inspecionar arte nova (6 prédios, 8 inimigos, 10 cenários).
- [ ] Estado/migração: sistemas da cidade, talentos, contratos, energia, relíquias, campanhas e percursos.
- [ ] Telas e entradas físicas da cidade.
- [ ] Modos com regras reais de combate e escolhas entre etapas.
- [ ] TypeScript/build e entrega.

### Estado anterior
- Perfil, 250 conquistas, 12 ranks de maestria, equipamentos e skins implementados; tsc/build passaram.
- 106 VFX e 20 fundos gerados na fase H2, parte ainda sem uso no código.
- Último E2E anterior falhou no painel pós-luta; rodada posterior interrompida. Não retomar.
- As regras históricas de proibir arte nova abaixo foram substituídas: nova arte IA autorizada.

## Histórico (fases A–G3)
ver roadmap.md — cards, parallax, exploração, 256 robôs, mineração, laboratório,
pets, cards/gadgets 100x, batalha 100x (100 animações + 28 VFX + 11 arenas).

## FASE H1 — Batalha 100x mais profunda (EM ANDAMENTO)

Pedido do usuário (2026-09-30):
- aprofundar o combate 1v1 por turnos **E** criar modos novos por cima
- **cada um dos 256 robôs com ataques e defesas escritos à mão**, nenhum repetido
- sistemas marcados: passivas/talentos, elementos com fraqueza/resistência/absorção,
  status avançados, timing (parry/esquiva/carga), combos + ultimate combinado,
  IA tática com dificuldades, chefes multi-fase, terreno com hazards, raid/sobrevivência,
  log detalhado + análise pós-luta, UI de combate nova
- entrega: só web por enquanto

### Checklist
- [x] recon do repo (battle.ts, skills.ts, robots.ts, engin/config, save.ts, battle.tsx)
- [ ] game/status.ts — 36 status avançados com mods/tick/reações
- [ ] game/elements.ts — 18 famílias + matriz de afinidade (fraco/resiste/imune/absorve)
- [ ] game/skills.ts — Skill estendida (tags, mecânicas, status, carga, recurso)
- [ ] game/kits/types.ts + index.ts — DSL autoral
- [ ] game/kits/b01..b08.ts — 256 kits autorais (32 por arquivo)
- [ ] game/passives.ts — ~26 hooks + passivas/talentos de cada robô
- [ ] game/mechanics.ts — ~60 mecânicas nomeadas resolvidas no motor
- [ ] game/terrain.ts — terreno/zona/hazard por arena
- [ ] game/combat.ts — motor profundo (iniciativa, formação, timing, combos, fases, IA)
- [ ] game/raid.ts — modo raid/sobrevivência com ondas
- [ ] screens/battle.tsx — UI nova (ordem de turno, preview de dano, fraquezas, combo, log)
- [ ] screens/codex.tsx — arquivo de combate (256 kits navegáveis)
- [ ] fiação: save.ts (formação/raid/loadout 6), menu, game-shell
- [ ] typecheck + build + E2E + deliver website

### Regras mantidas
- NENHUMA arte de personagem nova (só composição/recoloração + 28 VFX existentes)
- 4 fps idle / 8 fps ações; `image-rendering: pixelated`
- salvar em localStorage via save.ts



## Fase G2 — Cards e gadgets 100x (concluída)

### Cards (game/cards.ts, screens/cards.tsx)
- [x] 30 cards base × **7 selos** (BRUTO, RUBRO, POLAR, VOLTAICO, UMBRAL, AUREO, PRISMA) = **210 cards**; cada selo multiplica os mods, adiciona bônus/tag próprios e sobe raridade, preço e nível exigido
- [x] Selos recolorem a arte base por filtro CSS (cardFilter: sepia+saturate+hue-rotate+brightness) — **nenhuma arte nova gerada**, respeitando a ordem de não gerar personagem
- [x] **Refino +0..+9** por card (CARD_MAX_REFINE 10): custo em ouro escalando e **lingotes da mina** nos marcos, multiplicando os mods (cardModsAt/refineMult)
- [x] Slots de deck 3 → **5** (CARD_SLOT_LEVELS 1/1/1/20/45) e orçamento de energia crescendo com o nível (deckBudget)
- [x] Sinergias em **3 camadas**: por tag, por selo e **17 combos nomeados**; nearSynergies sugere o que falta
- [x] IA usa decks selados e refinados (aiDeck + aiRefineMap), escalando com o nível do adversário
- [x] Tela refeita: lista agrupada por base com contador de selos possuídos (N/7), modal com grade dos 7 selos, mods somados, comprar/equipar/remover e **refinar**
- [x] Correção de UI: clique dentro do modal não fecha mais o modal (o backdrop só fecha no toque fora)

### Gadgets (game/gadgets.ts, screens/engineer.tsx)
- [x] Nível máximo 10 → **30**, em 3 marcos **MK-I / MK-II / MK-III** (caps 10/20/30); cruzar marco cobra **lingotes**
- [x] **3 sockets** por gadget, liberados em Lv5/15/25 (gadgetSockets)
- [x] **48 chips** forjáveis (12 tipos × 4 tiers BRUTO/REFINADO/OTIMIZADO/SINGULAR), custando ouro + lingotes; gadgetTotals soma gadget + chips
- [x] IA também equipa chips (aiChips) conforme o nível
- [x] Tela com duas abas (GADGETS / FORJA DE CHIPS), etiqueta MK, fileira de sockets clicáveis, picker de chip e efeitos somados na ficha

### Save e batalha
- [x] save.ts: novos campos cardRefine / chips / gadgetChips com migração do save antigo; refineCard, buyChip, equipChip, gadgetChipsOf, cardSlotCount, gadgetBonusTable
- [x] battle.ts: Fighter.gadget → **gadgetLevel + gadgetMk + gadgetMods** (gadget + chips somados); deck do jogador e da IA resolvidos já com refino

### Verificação
- [x] typecheck --force OK, build OK
- [x] assets/e2e_cards_g2.py OK — 210 cards, 30 tiles, 7 miniaturas de selo com 7 filtros distintos, compra do bruto + do selado, equipar, refinar até +3 cobrando lingotes no marco, forja de chip, upgrade de gadget Lv10→11 virando MK-II com lingotes, chip encaixado no socket; 0 erros de console, 0 imagens quebradas, 0 HTTP>=400
- [x] assets/e2e_battle_g2.py OK — gadget MK-II sozinho +49.8% de força, com 2 chips T4 vai a **+67.3%** e ganha +10% de crítico; card selado rubro sai de +12.5% para **+20.4%** de poder com refino +7; força do lutador 131 → 219
- [x] assets/e2e_battle.py OK (partida real sem erros de console nem sprites quebrados)

### Próxima fase: G3 — batalha 100x com 100 animações novas

## Fase G3 — Batalha 100x, 100 animações, novos VFX e arenas animadas (concluída)

### Catálogo de animações (game/anim.ts — novo)
- [x] **100 AnimSpec** nomeadas em 5 camadas (`sprite`, `aura`, `burst`, `screen`, `float`), cada uma com keyframe, duração, VFX, filtro de cor, escala e nível de tremor
- [x] **20 famílias elementais de impacto** (`burst_fogo`, `burst_gelo`, `burst_luz`, …) resolvidas a partir do elemento do chassi — os 157 elementos dos robôs caem em 20 bursts distintos, sem nenhum id inválido
- [x] Resolvedores determinísticos: `burstAnimFor` (elemento), `hitAnimFor` (dano relativo + crítico → 5 faixas: leve/médio/pesado/crit/mega), `windupAnimFor` (preparo por skill, 6 gestos + carga de MP alto)
- [x] 64 novos `@keyframes mk-a-*` em styles.css + classes base `.mk-a`/`.mk-a-smooth` e `mk-shake-2`/`mk-shake-3`

### Instrumentação do combate (game/battle.ts)
- [x] Nova variante de evento `{ t: "anim"; side; id; hold }` — `hold 0` empilha animações em paralelo, permitindo sprite + aura + burst + tela simultâneos
- [x] **70 pontos de disparo** cobrindo `doAttack`, `doDefense`, `doItem`, `doSwap`, `runAction`, `tickEffects`, `handleFaints`, `checkEnd`, `resolveTurn`, `applyForcedSwap`
- [x] battle.tsx: `AnimInst`, fila com `hold`, `Stage` reescrito, novos componentes `AnimVfx` (por lutador) e `ScreenFx` (tela cheia), tremor de tela em 3 níveis
- [x] Menu de golpes virou lista rolável (máx. 148px) para não cobrir o palco durante as animações

### Novos VFX de ataque (20 novos, 28 no total)
- [x] 5 folhas 4×4 geradas em fundo magenta e processadas por `assets/process_vfx_g3.py` (chroma-key → alpha, tiras de 4 frames 96×96, padrão idêntico aos 8 originais): frost, toxic, cyclone, quake, torrent, prism, plasma, clang, bloom, cosmos, sonic, chrono, siphon, ward, laser, meteor, inferno, shockwave, buffup, curse
- [x] Chroma-key endurecido (`r>180 & b>180 & g<95 & |r-b|<70`) — o limiar antigo comia os roxos legítimos de toxic/cosmos/curse/prism/chrono/siphon
- [x] skills.ts: `ELEMENT_VFX` com 21 grupos de palavras-chave PT-BR + `vfxForElement` (fallback por hash) — os tiers `pierce/siphon/overdrive/ultimate` passaram a usar laser/siphon/buffup/meteor
- [x] `matchKeyword` compartilhado: palavras de até 4 letras só casam palavra inteira, então "SOM" não captura mais "SOMBRA" nem "AR" captura "AREIA"

### Arenas animadas em spritesheet único (11 no total)
- [x] 6 arenas novas geradas (neodojo, sunken, canyon, foundry, crystal, storm) + as 5 antigas migradas — **todos os 4 frames numa única imagem** `{arena}_sheet.png` (480×1088)
- [x] `ArenaBackdrop` reescrito: rola o sheet no eixo Y por `translateY` a cada 250ms, em vez de trocar 4 arquivos separados; os 20 PNGs `_f1..\_f4` foram removidos (10MB → 7.1MB) e o manifesto de carregamento foi atualizado
- [x] `game/arenas.ts` (novo): `ARENA_IDS`, `ARENA_NAMES`, `arenaForElement` (11 grupos temáticos) e `arenaForKey` (hash estável)
- [x] As 11 arenas entraram em rotação de verdade: `ARENAS` (modos), `RANKED_ARENAS` (ranqueada) e os **220 torneios** agora derivam a arena do elemento do torneio, em vez de 88 deles ficarem fixos em "dojo"
- [x] **Correção do bug relatado**: o vídeo de round era desenhado opaco por cima da arena com `objectFit: cover`, então só 2 fundos apareciam (`round % 5` quase sempre caía em round1/round2 nas lutas curtas). Agora ele é uma camada atmosférica (`opacity 0.22`, `mixBlendMode: screen`), a escolha mistura arena + round (todos os 5 entram em rotação) e há scanlines de CRT sobre tudo

### Verificação
- [x] typecheck --force OK (3/3), build OK
- [x] `assets/e2e_battle_g3.py` OK — 100 animações sem id duplicado, 157 elementos resolvidos em 20 bursts, 5 faixas de impacto distintas, 28 VFX todos 200/384×96, 11 sheets todos 200/480×1088, 60 turnos simulados gerando **893 eventos de animação / 31 ids distintos e nenhum fora do catálogo**; 0 erro de console, 0 pageerror, 0 imagem quebrada, 0 HTTP>=400
- [x] `assets/e2e_battle.py` OK (regressão: partida real sem erros nem sprites quebrados)

### G4 — Mundo dos Torneios: base explorável entregue
- [x] `WorldMap` substitui o mapa antigo na tela de torneios; fluxo prédio → preparação → batalha → relatório preservado.
- [x] 2860×640 tiles de 32px (91.520×20.480 px), área 100× a do mapa antigo; 12 biomas, 3 estratos subterrâneos, cavernas, poços e ilhas com plataformas.
- [x] 205 torneios físicos (superfície, câmaras e ilhas); soleiras protegidas das escavações vizinhas.
- [x] 62 assets processados: 14 atlases de chão, 14 de decoração, 28 fundos em camadas, 4 atlases de prédios, criaturas e itens; gerados por IA. Os 256 robôs permanecem intactos.
- [x] Andar, correr, salto duplo, golpe, pisão, dano, retorno à lanterna; 735 criaturas, 6667 decorações, 5344 itens e 95 lanternas.
- [x] Minimap com neblina de exploração e teleporte às lanternas acesas; pausa da física ao abrir; progresso e checkpoint persistentes.
- [x] Correção visual: paralaxe acompanha o bioma atual, sem atrasar um bioma; inspecionados início, selva, catacumbas, ilha e região nebular.
- [x] Vite fixado em 4200 no config (o projeto legado não possui `__ports.cjs`); servidor reiniciado para eliminar cache antigo.
- [x] E2E G4: todos os 205 acessos com chão e porta livres, 62 imagens decodificadas, movimento, salto, minimapa/pausa, entrada no torneio e retorno; zero console/pageerror/HTTP>=400/imagem quebrada.
- [x] Regressão `e2e_battle.py`: partida real e turno sem erro nem imagem quebrada.
- [ ] Puzzles e NPCs de exploração (não incluídos nesta base).
- [ ] G5: joystick analógico, ataque e especial; sem botão de correr.

### Expansão G4/G5 — pedido aprovado (entrega única)
- Escopo: mais 100× sobre 2860×640; geração de chunks sob demanda, 205 biomas derivados individuais, céu alto e subsolo profundo com rotas alcançáveis.
- Controles: joystick multitoque proporcional, ataque, especial com cooldown, pulo e interação; sem botão de correr; compartilhar com hub.
- Atividades: missões de coleta/caça/exploração, desafios de percurso e sobrevivência, batalhas rápidas reais, relíquias e postos de reparo; recompensas persistentes.
- Economia: reutilizar os 62 assets IA e sprites existentes; só gerar imagens se faltar uma função visual que a composição não resolva.
- Arquivos: world.ts (chunks/rotas/regiões/save), world-map.tsx (física/render/atividades), exploration-controls.tsx (novo), world-activities.ts (novo), tournaments.tsx (batalhas rápidas), hub-city.tsx (controles), testes de rotas e E2E.
- Sem novas APIs ou tabelas: progresso offline com migração não destrutiva do save G4; entrega web e mobile após typecheck/build/testes.
- [ ] Implementação em andamento.
## 2026-10-07 — PvP integrado + pedido de publicação na Play Store
- OnlineScreen integrada (título "ONLINE · PvP", dock PILOTO, guia). Redirect do login gerenciado concluído no Provider.
- Servidor recusa ações durante golpe carregado (só pass). Replay online com teto (3 revisões/140 eventos).
- Testes: API real 49 ✓, motor competitivo (48 partidas 1v1–4v4, simetria, travas) ✓, regressões 10 ✓, build ✓, HTTP 4200 root/health/auth ✓.
- Pendente: Google login não verificado; .env veio do scaffold mechach-hkul1er (app = campeoe-xwr5e13); exclusão de conta exigida pela Play Store não implementada.
- Play Store: publicar site → publicar mobile pelo painel do preview (conta Expo). Nunca buildar no sandbox.

## Bustos animados (2026-11-08) — FEITO
- 283 tiras em public/robots/bust, RobotBust no palco do hangar, E2E assets/e2e_bust.py PASS.
- Pendente opcional: 133 robôs com 9 frames (regerar para 12 se o usuário quiser).
