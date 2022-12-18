## 1. Adicionar este trecho ao arquivo go.yml
```yaml
deploy_EC2:
    needs: build
    uses: ./.github/workflows/ec2.yml
    secrets: inherit
```

## 2. Criar o arquivo ec2.yml

```text
Devemos sempre tomar cuidado ao trazermos um artefato para a nossa rotina, já que se os arquivos do artefato tiverem o mesmo nome de um arquivo da aplicação ele pode substituir o arquivo, e o mesmo também pode ocorrer se trouxermos a aplicação depois do artefato.

Então se tivermos um arquivo main.go dentro de um artefato e também no nosso repositório, com conteúdos diferentes, o último que chegar na nossa rotina irá substituir o primeiro.Então, se trouxermos a rotina com o actions/checkout@v3 e depois o artefato com `actions/download-artifact@v3.0.0`, a versão dele vai ser a que estará dentro da rotina.

Outro ponto importante é que até o actions/checkout@v2 todos os arquivos eram deletados da pasta de trabalho durante a sua execução e isso foi levantado como uma vantagem, então para garantir, sempre que você precisar executar um código que precise do actions/checkout, dê preferência para executá-lo como a primeira tarefa.
```

## 3. Criar os secrets
- SSH_PRIVATE_KEY: basta colar no value o codigo da chave que realizamos download quando configuramos a VM
- REMOTE_HOST: No painel EC2 > Instancias > Copiar o Endereco IPv4 Publico
- REMOTE_USER: No painel EC2 > Instancias > Seleciona a instancia > Conectar > Aba Conexao de Instancia do EC2 > Nome do usuario

- DBHOST: No painel do RDS > databases > Seleciona a instancia > Copiar o endpoint, la tambem temos as info das outras var de bancos de dados

> Lembrar de criar os secrets que ja tinhamos
- PASSWORD_DOCKER_HUB: 

```text
Ambos os comandos nohup e & são muito conhecidos em servidores linux, porém não são tão comuns para usarmos nos desktops.

nohup
O nohup é usado para separar o comando depois dele do terminal, mas para que separar um comando do terminal? Eu não interajo com o comando pelo terminal?

Separamos o comando do terminal para que ele continue executando mesmo se o terminal fechar, podemos fazer um teste, se você está em uma máquina linux, você pode iniciar um programa, como o navegador, através de um comando, para o Firefox o comando é firefox e para o google chrome é google-chrome, assim que o navegador abrir, feche o terminal e você verá que o navegador também vai fechar.

Agora coloque o nohup na frente do comando, ficando com nohup firefox ou nohup google-chrome, assim que o navegador abrir, feche o terminal e você verá que o navegador não irá fechar.

O mesmo ocorre em uma conexão SSH, quando fechamos a conexão, o terminal usado também é fechado e as aplicações sendo executadas nele são finalizadas.

&
O & permite que o comando seja executado em segundo plano, assim o programa não fica aberto no nosso terminal e permite com que seja possível usar o mesmo terminal para outros comandos.

Então para a nossa aplicação, é importante executar ela com o & assim o terminal é liberado, e para todos os efeitos, o terminal identifica que o programa terminou de executar e pode ir para o próximo comando, ou finalizar o script.

Sem o & no final do comando, vamos encontrar que o nosso script não finaliza enquanto a aplicação estiver em execução.

```


```text
No Linux podemos redirecionar as saídas e entradas dos comandos para arquivos ou outros comandos, utilizando o símbolo de maior que >, e o de menor que <.

O símbolo de maior que serve para redirecionar as saídas, podendo ser as saídas de log ou as saídas de erros, enquanto o símbolo de menor que redireciona as entradas.

No nosso caso temos o comando nohup ./main > nohup.out 2> nohup.err < /dev/null &, então temos que os logs vão para o arquivo nohup.out, os erros vão para nohup.err e se o programa precisar de alguma entrada usamos o /dev/null para não mandamos nada.

Se fosse python -> nohup python API.py > api.log 2>&1 &
Vamos ter a saída de logs e erros para o arquivo apt.log e vamos ter a aplicação funcionando sem problemas, mesmo quando a rotina terminar de executar
```

## 4. Realizar o commit e deploy no git actions
