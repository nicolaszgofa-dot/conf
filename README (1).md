# Configurador J.Assy — v0.2

Protótipo do configurador técnico de equipamentos (Titanium Electric / Selenium Electric),
baseado na especificação v0.1. Front-end puro (HTML + CSS + JS), sem dependências e sem build step.

## Estrutura

```
jassy-configurador/
├── index.html   # marcação e wizard (telas 1–9)
├── style.css    # estilos
├── app.js       # estado do wizard + motor de regras
└── README.md
```

## Motor de regras (v0.2)

Lógica isolada em `app.js`, pronta para extração a um backend:

- `computeDistribution()` — as 6 saídas do alternador são um **recurso global**
  (R012/R013): as linhas da máquina são distribuídas em até 10 por saída, sem
  considerar seções. Ex.: 55 linhas → 10/10/10/10/10/5.
- `computeComponents()` — ECU (1), sensor de levante (1), sensor de semente
  (= total de linhas), ISOBUS, alternador ou cabo de bateria.
- `buildConfiguration()` — gera o JSON estruturado da configuração.

Seções são validadas (soma = total) e armazenadas, mas não influenciam as saídas.
Cabo de painel (4/9/14 m) e cabos de linha (códigos 30000000687 / 30000000688) são
apenas reconhecidos; as regras de escolha estão **pendentes** e não foram inventadas.
Preço, estoque, marca/modelo e ERP ficam fora desta versão.

## Rodando localmente

Não precisa de servidor nem instalação — é só abrir `index.html` num navegador.

Se preferir servir por HTTP (recomendado para evitar bloqueios de `file://` em
alguns navegadores):

```bash
cd jassy-configurador
python3 -m http.server 8000
# depois abra http://localhost:8000
```

## Publicando para acesso externo

Qualquer hospedagem de arquivos estáticos serve, já que não há backend:

### GitHub Pages
1. Crie um repositório e suba estes 3 arquivos (`index.html`, `style.css`, `app.js`).
2. Em *Settings → Pages*, selecione a branch principal como fonte.
3. O app fica disponível em `https://<usuario>.github.io/<repositorio>/`.

### Netlify / Vercel
1. Arraste a pasta `jassy-configurador` na área de deploy do painel (ou conecte o
   repositório Git).
2. Nenhuma configuração de build é necessária — é site estático puro.

### Servidor próprio (Nginx/Apache/IIS)
Copie os 3 arquivos para a pasta pública do servidor (`/var/www/html`, `wwwroot`, etc.).

## Próximos passos (fora do MVP)

- Definir e implementar as regras pendentes R01–R13 (comprimento de cabos,
  mangueiras, banco de marcas/modelos/adaptações, etc.).
- Adicionar as etapas de Marca → Modelo → Adaptação → Configuração final.
- Extrair o motor de regras para uma API própria, com persistência em banco e
  geração de pedido de venda.
