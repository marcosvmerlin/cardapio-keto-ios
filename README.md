# Cardápio Keto — app iOS

O app web "Cardápio Keto do Marcos" embrulhado como app de iPhone (Capacitor), com lembretes de remédios e água, backup e um resumo do dia pronto para um widget futuro. Instala-se pelo AltStore com um Apple ID comum, sem pagar conta de desenvolvedor.

## O que está aqui

| Pasta/arquivo | Para quê |
|---|---|
| `www/index.html` | O app inteiro (página única). É o que o iPhone mostra. |
| `capacitor.config.json` | Nome, identificador (`com.marcos.cardapioketo`) e cor de fundo. |
| `ios/` | Projeto Xcode gerado pelo Capacitor (ícone, Info.plist, entitlements do App Group). |
| `.github/workflows/ios.yml` | Compila o app numa máquina Mac do GitHub e gera o `.ipa`. |
| `Cardápio Keto do Marcos.html` | Cópia da versão web original, sem as funções nativas. |

## Passo a passo

### 1. Subir o projeto para o GitHub

1. Crie uma conta em github.com, se ainda não tiver.
2. Crie um repositório novo, **público** (as máquinas Mac do GitHub são gratuitas só em repositórios públicos). Nome sugerido: `cardapio-keto-ios`. Não marque "Add a README".
3. No PC, dentro desta pasta, rode no terminal (troque `SEU-USUARIO`):

```bash
git remote add origin https://github.com/SEU-USUARIO/cardapio-keto-ios.git
```

```bash
git push -u origin main
```

O GitHub vai pedir login na primeira vez.

### 2. Gerar o `.ipa`

1. No site do repositório, abra a aba **Actions**.
2. O fluxo "Gerar IPA (iOS, sem assinatura)" roda sozinho a cada `push`. Se não rodou, clique nele, depois em **Run workflow**.
3. Leva uns 10 a 15 minutos. Quando ficar verde, clique na execução e, no fim da página, em **Artifacts**, baixe `Cardapio-Keto-ipa`. É um `.zip` com o `Cardapio-Keto.ipa` dentro; extraia.

### 3. Instalar o AltStore

1. No PC (Windows): baixe e instale o **AltServer** em altstore.io. Ele fica na bandeja do sistema, perto do relógio. Precisa do iTunes e do iCloud da Apple instalados (versões do site da Apple, não da Microsoft Store), como o site do AltStore explica.
2. Conecte o iPhone no PC pelo cabo, clique no ícone do AltServer na bandeja › **Install AltStore** › escolha o iPhone. Entre com o seu Apple ID e senha.
3. No iPhone: Ajustes › Geral › VPN e Gerenciamento de Dispositivo › confie no seu Apple ID.

### 4. Instalar o Cardápio Keto

1. Mande o `Cardapio-Keto.ipa` para o iPhone (AirDrop não existe no Windows; use iCloud Drive, Google Drive, WhatsApp "para mim" ou e-mail).
2. Abra o **AltStore** no iPhone › aba **My Apps** › toque em **+** no canto superior › escolha o `.ipa`.
3. O AltStore assina o app com o seu Apple ID e instala. O ícone "Cardápio Keto" aparece na tela de início.

### 5. Renovação dos 7 dias

Com Apple ID gratuito, a Apple exige reassinar o app a cada 7 dias. O AltStore faz isso sozinho quando o iPhone está na mesma rede Wi-Fi do PC com o AltServer aberto. Deixe o AltServer iniciar com o Windows e, pelo menos uma vez por semana, tenha o iPhone na mesma rede. Se vencer, o app não abre até renovar, mas os dados continuam guardados; abra o AltStore › My Apps › **Refresh**.

Limite da conta gratuita: 3 apps instalados assim ao mesmo tempo (o próprio AltStore conta como um).

### 6. Lembretes

Dentro do app, na aba **Rotina**, o cartão "Lembretes no iPhone" aparece só na versão instalada. Toque em **Ativar lembretes**, escolha os grupos (manhã, almoço, tarde, pré-treino, jantar, noite, água) e salve; o iPhone pede permissão na primeira vez. "Testar em 5 s" dispara um aviso de teste.

Os horários seguem a rotina do app: 6:40 (seg, qua e sex) ou 7:25 (outros dias), 12:00, 15:30, 16:45, 19:40, 21:30, e água às 10:00, 14:00 e 17:30. Se mudar a rotina no código, reabra "Ajustar lembretes" e salve de novo.

### 7. Backup

Na aba **Rotina**, cartão "Backup":

- **Exportar backup** cria um arquivo `cardapio-keto-AAAA-MM-DD.json` e abre a folha de compartilhar do iPhone: salve em Arquivos/iCloud ou mande para você mesmo. Na versão web, o texto é copiado para a área de transferência.
- **Importar backup** abre uma janela para colar o conteúdo do arquivo; o app confere e pede confirmação antes de substituir os dados atuais.

Faça um backup antes de apagar o app ou trocar de iPhone. Os dados ficam só no aparelho: a versão instalada não sincroniza com a versão web do claude.ai.

## Atualizar o app depois de mudar algo

1. Edite `www/index.html`.
2. `git add -A`, `git commit -m "o que mudou"`, `git push`.
3. Baixe o novo `.ipa` nos Artifacts e instale pelo AltStore por cima do anterior (os dados ficam).

## Widget (futuro)

A cada mudança, o app grava um resumo do dia (kcal, próxima refeição, próximo remédio, água) no App Group `group.com.marcos.cardapioketo`, pela chave `resumo`. Um widget em WidgetKit pode ler esse valor com `UserDefaults(suiteName:)`. O entitlement já está no projeto; o AltStore ajusta o nome do grupo na assinatura.

## Se der erro

- **Actions vermelho em "Compilar sem assinatura"**: abra o log; na maioria das vezes é uma versão nova do Xcode no GitHub. Rode de novo com **Re-run jobs** antes de mexer em algo.
- **AltStore diz que atingiu o limite de apps**: remova um app assinado pelo AltStore ou use a conta paga.
- **Lembretes não chegam**: Ajustes › Notificações › Cardápio Keto › permitir; e Ajustes › Foco não pode estar silenciando.
