# provisionarCisco7942

<h1>Como provisionar telefone Cisco 7942 com linhas SIP</h1>



Nesse artigo vou mostrar um breve tutorial sobre como você deve provisionar seu telefone Cisco 7942, com suas linhas SIP.  O Cisco 7942 possui 2 linhas e você pode configurar somente uma ou as duas.  É importante salientar que neste artigo, não estou mostrando como converter o telefone de SCCP para SIP, mas sim apenas como provisioná-lo, ou seja, como configurá-lo com suas linhas voip SIP, seja ramais de um servidor de sua rede (Asterisk, 3CX, etc) ou de algum provedor externo na web.

O provisionamento não é difícil de fazer, basta seguir os passos abaixo. Também estou anexando a esse artigo, um vídeo que fiz e está disponível no Youtube (no vídeo mostro apenas a configuração no telefone, portanto a configuração do arquivo XML é mostrada aqui).



<h2>INFORMAÇÕES IMPORTANTES, LEIA ANTES DE INICIAR:</h2>
 

- A configuração do seu telefone é de sua responsabilidade. Não me responsabilizo por qualquer problema ou dano que seu telefone apresente após a configuração. Relaxe, não precisa apavorar, até porque que o arquivo XML que você vai subir para o telefone, não tem nada demais, só configurações básicas - você vai abrir e ver - mas não me responsabilizo por qualquer problema ok? Ao realizar o procedimento está fazendo por sua conta e risco.

- Tenha em mente que você estará subindo para o telefone, via TFTP, um arquivo de configuração, então atente-se às configurações para que estejam corretas e, se possível, conecte o computador e o telefone ip em um nobreak (para evitar que uma queda de energia durante o procedimento não danifique seu telefone).

- Nessas instruções abaixo eu estou recomendando um software para windows, o Pumpkin TFTP Server, se você é um usuário Linux ou Mac, é semelhante, mas você precisará instalar o servidor TFTP conforme seu sistema operacional.

- Muito importante: No computador onde você instalar o Pumpkin, desative qualquer firewall que esteja ativo, temporariamente, enquanto você faz a transferência do arquivo de configuração. Seja o firewall do Windows mesmo ou outro instalado, desative-o, senão vai dar erro e o telefone não vai se conectar ao computador.

- Vou mostrar somente como alterar as tags para configuração da sua linha SIP ok? As demais tags você pode deixar como estão ou então mudar conforme sua preferência. Se você é leigo, não mexa em nada, melhor.. rsss  O arquivo dialplan.xml altere como preferir. Se não quiser mexer nele, suba como está, funcionará.

- Se as configurações do seu telefone estiverem travadas, ou seja, sem opção de edição, ao entrar no menu SETTINGS, aperte * * #  no telefone que vai destravar e deixar você editar.



<h3>PASSO 1 -  OBTENDO INFORMAÇÕES NO SEU TELEFONE</h3>


1 - Primeiro de tudo: conecte seu telefone Cisco 7942 à sua rede e ligue-o na energia. Não é preciso conectar o telefone com um cabo diretamente no computador não, basta conectar ele na sua rede atual.

2 - Agora, o que você precisa fazer é obter o endereço MAC e o firmware (loadfile) do seu telefone Cisco 7942. Para isso, no seu telefone, aperte o botão SETTINGS e depois em MODEL INFORMATION.  Na tela serão exibidos o MAC e o LoadFile. Anote-os no seu computador, você precisará dessas informações para inserir no arquivo de configuração XML.

3 - Agora com o telefone já ligado, aperte SETTINGS e NETWORK CONFIGURATION e IPV4 CONFIGURATION.  Em DHCP, se já estiver ativo, seu roteador já deve ter atribuído um ip ao telefone, pode manter como está. Se preferir ou se o DHCP estiver desativado (disabled), então coloque um ip fixo no seu telefone, dentro da sua faixa de ips da sua rede, claro. Neste caso se for informar um ip fixo, você precisa também informar a máscara da rede, o router (default router 1) e dois DNS.

4 - Ainda em IPV4 CONFIGURATION, desça até o item TFTP SERVER 1. Esse é muito importante, você vai colocar aí, o endereço ip do computador onde for instalar o Pumpkin TFTP Server. Mas não insira agora, faça o procedimento descrito no passo 2 abaixo, para no final inserir o endereço ip TFTP SERVER 1 no telefone. Isso porque, quando você inserir o ip e salvar, o telefone já vai se conectar ao computador (TFTP SERVER) para buscar os arquivos, automaticamente, não sendo necessário reiniciar o telefone ok?

 
 
<h3>PASSO 2 - CONFIGURAR ARQUIVO XML</h3>


1 - Baixe o arquivo zip no link e descompacte-no seu computador:   Baixar arquivo aqui

2 - Renomeie o arquivo  SEP[MACDOSEUTELEFONE].cnf.xml  incluindo o endereço MAC do seu telefone onde está [MACDOSEUTELEFONE], em caixa alta. Deverá ficar assim, ex: SEP54AB3E5D39C7.cnf.xml  (esse MAC é fictício, informe o endereço MAC do seu aparelho).

3 - Agora abra o arquivo usando o bloco de notas ou algum editor de sua preferência (ex: Notepad++, Brackets, etc). 

4 - Localize a tag "processNodeName" e dentro dela, informe o endereço ip do seu servidor Asterisk ou do seu provedor voip.  

5 - Agora desça até a linha que contém a tag "loadInformation" e dentro dela, informe a versão do firmware obtido no seu telefone (Loadfile). É importante informar exatamente como está no seu telefone, case, traços, pontos, etc. senão não vai funcionar.

6 - Agora localize a tag "sipLines" e dentro dela, você vai localizar as tags "line button=1" e "line button=1", que é a configuração de cada uma das linhas. Se você só vai configurar uma linha, apague então a tag "line button=1" até o fim "/line".

7 - Preencha os itens da tag "line button=1" da seguinte forma (o que eu não mostrar, é só não mexer):

<b>featureLabel</b> - Coloque ali o nome que você quer que seja exibido na frente da linha, no visor do telefone.<br />
<b>name</b> - Informe o usuário do seu ramal ou linha voip.<br />
<b>displayName</b> - Informe o usuário do seu ramal ou linha voip.<br />
<b>contact</b> - Informe o usuário do seu ramal ou linha voip.<br />
<b>proxy</b> - Informe o endereço ip do seu servidor Asterisk ou provedor voip.<br />
<b>port</b> - Informe a porta sip do seu servidor/provedor (em geral é 5060 mesmo, se você não tiver essa informação, mantenha 5060).<br />
<b>authName</b> - Informe o usuário do seu ramal ou linha voip.<br />
<b>authPassword</b> - Informe a senha do seu ramal ou linha voip.<br />

Repita o preenchimento das tags no "line button=2", se você for configurar duas linhas. Agora salve o arquivo e feche-o.

8 - Seu arquivo está pronto. Agora é só fazer a transferência para o telefone e provisioná-lo.  Siga o terceiro passo abaixo.


 
<h3>PASSO 3 - CONFIGURAR SERVIDOR TFTP SERVER</h3>


1 - Baixe o Pumpkin TFTP Server nesse link aqui ( Baixar Pumpkin )  ou de qualquer outro site que preferir.  Instale-o no seu computador e desative o firewall do seu Windows.

2 - Abra o Pumpkin e clique no botão OPTIONS.

3 - Em "TFTP filesystem root (download path)" você vai selecionar, no seu computador, a pasta onde estão os arquivos XML (de configuração e o dialplan).

4 - Em "Read Request Behavior", marque a opção GIVE ALL FILES  e, em "Write Request Behavior", marque a opção "Take all files".

5 - Agora clique em OK. Pronto! O servidor TFTP server está pronto.  Agora siga o quarto passo para finalizar.


 
<h3>PASSO 4 - TRANSFERIR OS ARQUIVOS PARA O TELEFONE</h3>


1 - Agora que já está tudo pronto, no seu telefone ip, aperte o botão SETTINGS e vá em NETWORK CONFIGURATION > IPV4 CONFIGURATION  e desça até a opção TFTP SERVER 1. Aperte o botão EDIT e informe o endereço ip do computador onde está rodando o Pumpkin TFTP Server. 

2 - Ao apertar o botão VALIDADE e SAVE, automaticamente o telefone se conectará ao servidor TFTP e começará a transferir os arquivos. É tudo bem rápido e o Pumpkin vai mostrar na tela o log da transferência (e fazer uns barulhinhos também).

3 - Se você fez tudo certo, está pronto, seu telefone já não mostrará mais "unprovisioned" e já vai mostrar as linhas registradas. 

4 - Se as linhas não registrarem, cheque a configuração de usuário, senha, etc no seu servidor Asterisk e no arquivo de configuração.

 

Pronto, o provisionamento está finalizado!  Se tiver alguma dúvida em que eu possa ajudar,  mande para meu email  alvi@alaj.com.br .

 

Abs.

Alvi Jr
