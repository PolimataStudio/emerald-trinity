# Emerald Trinity — Câmera Institucional PWA

PWA de captura fotográfica e audiovisual com estampagem automática de
logomarca e descrição, projetado para publicação no GitHub Pages e
empacotamento Android via PWABuilder (TWA).

## Funcionalidades

- Câmera traseira/frontal com alternância quando suportada.
- Fotografia com logomarca (canto superior direito) e estampa de descrição
  (canto inferior esquerdo) incorporadas aos pixels.
- Gravação de vídeo com estampagem em tempo real (quando suportada) e
  fallback para gravação original.
- Configurações institucionais persistentes em IndexedDB.
- Funcionamento offline após a primeira visita.
- Exportação por download e compartilhamento nativo quando disponível.

## Estrutura

/
├── index.html
├── manifest.webmanifest
├── sw.js
├── offline.html
├── css/styles.css
├── js/{app,utils,storage,settings,camera,image-compositor,video-recorder}.js
├── assets/icons/{icon-192.png, icon-512.png, maskable-512.png}
├── .well-known/assetlinks.json
├── README.md
└── TESTING.md


## Execução local

1. Clone o repositório.
2. Sirva a pasta raiz com um servidor HTTPS local (obrigatório para câmera).
   Exemplo com Python:
   ```bash
   python3 -m http.server 8080

   Para HTTPS local, use mkcert ou ngrok.

Acesse http://localhost:8080 (a câmera pode exigir HTTPS;
em localhost alguns navegadores permitem).

Publicação no GitHub Pages
Crie um repositório no GitHub e envie os arquivos.

Em Settings → Pages, selecione a branch main e a pasta / (root).

Aguarde a URL https://<usuario>.github.io/<repo>/.

Verifique se o manifest e o Service Worker carregam sem 404.

PWABuilder e Android
Acesse PWABuilder.com.

Insira a URL pública do GitHub Pages.

Analise o relatório; corrija erros de manifest, ícones ou SW.

Clique em Package for stores → Android.

Preencha Package ID (ex.: br.org.associacao.emeraldtrinity),
App name, versão e cores.

Gere o pacote. O PWABuilder utiliza Bubblewrap para criar um TWA.

Requisitos: Android 7.0 (API 24+) e ícone 512×512 PNG.

Para publicar na Play Store, é necessária uma conta de desenvolvedor Google.

Configure o Digital Asset Links: hospede .well-known/assetlinks.json
com o package name e a impressão digital SHA-256 do certificado de assinatura
em https://<usuario>.github.io/.well-known/assetlinks.json (na raiz do domínio,
não no subdiretório do repositório, se aplicável).

APK vs AAB: APK é instalável diretamente; AAB é exigido pela Play Store
para novos apps. O PWABuilder oferece ambas as opções conforme o fluxo.

Privacidade
Nenhum arquivo é enviado a servidores externos.

Nenhum analytics ou rastreamento.

Permissões solicitadas apenas no momento do uso.

Dados internos podem ser apagados em Configurações → Limpar todos os dados.

Arquivos já baixados permanecem no dispositivo do usuário.


### 6.2. `TESTING.md`

```markdown
# Plano e Resultados de Testes

## Testes estáticos realizados

- [x] Manifest contém `name`, `short_name`, `start_url`, `scope`, `display`, `theme_color`, `icons`.
- [x] Ícones declarados correspondem a caminhos existentes (192, 512, maskable 512).
- [x] Service Worker registrado com escopo relativo.
- [x] Lista de pré-cache inclui todos os módulos JS e o CSS.
- [x] Nenhum caminho absoluto `/` no HTML ou manifest.
- [x] Tratamento de `NotAllowedError`, `NotFoundError`, `NotReadableError`.
- [x] Fallback de `ImageCapture` para canvas.
- [x] Verificação de `MediaRecorder.isTypeSupported()` antes de gravar.
- [x] Fallback de gravação estampada para gravação original.
- [x] Composição proporcional ao tamanho do arquivo final (não ao viewport).
- [x] IndexedDB utilizado para logomarca e configurações.
- [x] Botão de limpeza de dados com confirmação.
- [x] `prefers-reduced-motion` respeitado na animação de gravação.

## Testes pendentes (requerem dispositivo real)

- [ ] Instalação como PWA em Android/iOS.
- [ ] Acesso à câmera traseira e frontal.
- [ ] Captura de foto e verificação da estampa incorporada.
- [ ] Gravação de vídeo com estampagem em tempo real (desempenho e sincronização).
- [ ] Funcionamento offline após primeira visita.
- [ ] Atualização do Service Worker (nova versão assume o controle).
- [ ] Empacotamento no PWABuilder e instalação do APK/AAB.

## Como executar os testes pendentes

1. Publique no GitHub Pages.
2. Abra no Chrome Android.
3. Conceda permissões de câmera e microfone.
4. Capture uma foto e verifique no visualizador se a logomarca e a descrição
   estão nos cantos corretos e proporcionais.
5. Grave um vídeo curto (10–15 s) e verifique a reprodução.
6. Ative o modo avião após a primeira visita e recarregue a página.
7. No PWABuilder, gere o pacote e instale o APK em um dispositivo Android 7.0+.
